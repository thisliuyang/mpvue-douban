<template>
  <div class="box">
    <swiper class="slide" indicator-dots="true" autoplay="true" interval="5000" duration="1000">
      <block v-for="(item, index) in boards[0].movies" :key="index" >
        <swiper-item>
          <img :src="item.images.small" mode="scaleToFill" />
        </swiper-item>
      </block>
    </swiper>
    <div class="container_b" v-for="(item, i) in boards" :key="item.key">
      <a :href="'/pages/item/main?type='+ item.key +'&title=' + item.title">
        <div class="item_title">{{item.title}}</div>
      </a>
      <scroll-view class="content" scroll-x="true">
        <div class="inner">
          <a :href="'/pages/item/main?id='+ items.id" v-for="(items, j) in item.movies" :key="j" >
            <div class="movie-item">
              <img class="movie-item-img" :src="items.images.small" mode="scaleToFill" />
              <span>{{ items.title }}</span>
            </div>
          </a>
        </div>
      </scroll-view>
    </div>
  </div>
</template>
<script>
import { find } from '../../utils/api'
export default {
  data () {
    return {
      imgUrls: [
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/newsPicture/05558951-de60-49fb-b674-dd906c8897a6',
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/coursePicture/0fbcfdf7-0040-4692-8f84-78bb21f3395d',
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/management-school-picture/7683b32e-4e44-4b2f-9c03-c21f34320870'
      ],
      boards: [
        { key: 'in_theaters' },
        { key: 'coming_soon' },
        { key: 'new_movies' },
        { key: 'top250' }
      // { key: 'weekly' },
      // { key: 'us_box' }
      ]
    }
  },
  created () {
    wx.getLocation({
      type: 'wgs84',
      altitude: true,
      success (res) {
        console.log(res)
      }
    })
    // https://douban.uieee.com/v2/movie/in_theaters?start=0&count=8&city=%E5%8C%97%E4%BA%AC
    wx.showLoading({ title: '拼命加载中...' })
    const tasks = this.boards.map(board => {
      return find(board.key, 1, 8)
        .then(d => {
          board.title = d.title
          board.movies = d.subjects
          return board
        })
    })

    Promise.all(tasks).then(boards => {
      this.boards = boards
      wx.hideLoading()
    })
  }
}
</script>
<style scoped>
.box {
  background: #f8f9fb;
}
.slide {
  height: 480rpx;
}
img {
  width: 100%;
  height: 100%;
}
.movie-item {
  width:180rpx;
  display: flex;
  flex-direction: column;
}
.movie-item span {
  font-size: 28rpx;
  text-align:center;
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis;
}
.inner {
  display:flex;
  flex-direction:row;
  height:300rpx;
  width:900rpx;
}
.inner a {
  width:180rpx;
  margin: 5rpx;
}
.container_b {
  margin-top: 30rpx;
  background: #fff;
  padding: 0 10rpx;
}
.movie-item-img {
  width:180rpx;
  height:250rpx;
}
.item_title {
  font-size: 28rpx;
  padding: 10rpx;
}
</style>