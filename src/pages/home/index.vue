<template>
  <div>
    <!-- 头部 -->
    <div class="top-bar">
      <span>豆瓣</span>
      <icon type="search"
            size="24"
            color="#00b600"></icon>
      <button>打开豆瓣App</button>
    </div>

    <!-- 影院热映 -->
    <div class="content">
      <p class="title">
        <span>影院热映</span>
        <span class="more">更多</span>
      </p>

      <!-- 横向滚动 -->
      <scroll-view class="scroll-view_H"
                   scroll-x="true"
                   bindscroll="scroll"
                   style="width: 100%">
        <view v-for="item in theatherMovies"
              :key="item.id"
              class="scroll-view-item_H demo-text-1">
          <img :src="item.images.large"
               alt="">
          <p>{{item.title}}</p>
          <div class="rating">
            <div class="stars"
                 v-show="item.rating.average">
              <img v-for="(item2 , index2) in (item.starNum)"
                   :key="index2"
                   src="../../../static/images/star.svg"
                   alt="">
              <img v-for="(item2 , index2) in (5-item.starNum)"
                   :key="index2"
                   src="../../../static/images/unstar.svg"
                   alt="">
            </div>
            <span>{{item.rating.average?item.rating.average:"暂无评论"}}</span>
          </div>
        </view>
      </scroll-view>

    </div>

    <!-- top250 -->
    <div class="content">
      <p class="title">
        <span>top250</span>
        <span class="more">更多</span>
      </p>

      <!-- 横向滚动 -->
      <scroll-view class="scroll-view_H"
                   scroll-x="true"
                   bindscroll="scroll"
                   style="width: 100%">
        <view v-for="item in top250Movies"
              :key="item.id"
              class="scroll-view-item_H demo-text-1">
          <img :src="item.images.large"
               alt="">
          <p>{{item.title}}</p>
          <div class="rating">
            <div class="stars"
                 v-show="item.rating.average">
              <img v-for="(item2 , index2) in (item.starNum)"
                   :key="index2"
                   src="../../../static/images/star.svg"
                   alt="">
              <img v-for="(item2 , index2) in (5-item.starNum)"
                   :key="index2"
                   src="../../../static/images/unstar.svg"
                   alt="">
            </div>
            <span>{{item.rating.average?item.rating.average:"暂无评论"}}</span>
          </div>
        </view>
      </scroll-view>

    </div>

  </div>
</template>

<script>
export default {

  data () {
    return {
      // 影院热映
      theatherMovies: [],
      // top250
      top250Movies: []

    }
  },

  onLoad () {
    // 发送请求
    this.getTheaterMovies()
    this.getTop()
  },

  methods: {
    // 获取影院热映
    getTheaterMovies () {
      wx.request({
        url: 'https://api.douban.com/v2/movie/in_theaters?apikey=0df993c66c0c636e29ecbb5344252a4a',
        header: {
          'content-type': 'application/x-www-form-urlencoded'
        },
        success: res => {
          const { statusCode, data: { subjects } } = res
          if (statusCode === 200) {
            console.log(subjects)
            // 对评分进行处理
            subjects.forEach(item => {
              // 星星(发亮)的数量是评分向上取整
              item.starNum = Math.ceil(item.rating.average / 2)
            })
            this.theatherMovies = subjects
          }
        }
      })
    },
    // 获取top250
    getTop () {
      wx.request({
        url: 'http://api.douban.com/v2/movie/top250?apikey=0df993c66c0c636e29ecbb5344252a4a',
        header: {
          'content-type': 'application/x-www-form-urlencoded'
        },
        success: res => {
          const { statusCode, data: { subjects } } = res
          if (statusCode === 200) {
            console.log(subjects)
            // 对评分进行处理
            subjects.forEach(item => {
              item.starNum = Math.ceil(item.rating.average / 2)
            })
            this.top250Movies = subjects
          }
        }
      })
    }
  }

}
</script>

<style lang="less">
.top-bar {
  display: flex;
  align-items: center;
  border: 1px solid #eee;
  height: 94rpx;
  padding: 0 36rpx;
  span {
    font-size: 18px;
    color: #00b600;
  }
  icon {
    flex: 1;
    margin: 4rpx 0 0 28rpx;
  }
  button {
    width: 210rpx;
    height: 58rpx;
    background-color: #42bd56;
    color: #fff;
    font-size: 12px;
  }
}

.content {
  margin-bottom: 62rpx;
}

.title {
  height: 88rpx;
  line-height: 88rpx;
  display: flex;
  justify-content: space-between;
  padding: 0 38rpx;
  .more {
    color: #42bd56;
  }
}

.scroll-view_H {
  white-space: nowrap;
  margin-top: 12rpx;
}
.scroll-view-item_H {
  display: inline-block;
  margin-right: 18rpx;
  width: 200rpx;
  overflow: hidden;

  img {
    width: 200rpx;
    height: 286rpx;
  }
  p {
    margin-top: 20rpx;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    word-break: break-all;
    text-align: center;
  }
  .rating {
    display: flex;
    justify-content: center;
    margin-top: 6rpx;
  }
  .stars {
    img {
      width: 20rpx;
      height: 20rpx;
    }
  }
}
</style>