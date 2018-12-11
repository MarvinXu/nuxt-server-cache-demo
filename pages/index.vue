<template>
  <section class="container">
    <div>
      <logo/>
      <h1 class="title">
        nuxt-test
      </h1>
      <h2 class="subtitle">
        My spectacular Nuxt.js project
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green">Documentation</a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey">GitHub</a>
      </div>
      <ul class="nav">
        <li 
          v-for="(item, i) in mergedNav" 
          v-if="!item.hide" 
          :key="i"
        >
          <span class="text">{{ item.text }}</span>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
// 默认情况这里声明的变量，之后的请求不会重新初始化
// 设置runInNewContext:true之后，每次请求都会重新初始化
const DEFAULTS = [
  {
    text: '首页'
  },
  {
    text: '分类'
  },
  {
    text: '卖手机'
  },
  {
    text: '晒单'
  },
  {
    text: '找客服'
  }
]
let count = 0
export default {
  components: {
    Logo
  },
  async asyncData ({app}) {
    const {respData} = await app.$axios.$get('http://youpinapi.zhuaninc.com/goods/idx/footer')
    return {
      nav: respData.nav
    }
  },
  data () {
    return {
      // 每次请求重新初始化
      defaultNav: [
        {
          text: '首页'
        },
        {
          text: '分类'
        },
        {
          text: '卖手机'
        },
        {
          text: '晒单'
        },
        {
          text: '找客服'
        }
      ],
      count: 0
    }
  },
  computed: {
    mergedNav() {
      // 这个操作修改了global DEFAULTS，导致server和client渲染不一致
      const res = DEFAULTS.map((item, index) => Object.assign(item, this.nav[index]))
      // const res = this.defaultNav.map((item, index) => Object.assign(item, this.nav[index]))
      console.log('mergedNav: ', res)
      return res
    }
  },
  created () {
    console.log('global count: ', count++)
    console.log('this.count: ', this.count++)
  }
}
</script>

<style>

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
