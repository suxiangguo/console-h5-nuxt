<template>
  <div class="panel">
    <div class="layui-container">
      <ul class="layui-clear">
        <nuxt-link
          v-for="(item, index) in navList"
          :key="'nav' + index"
          tag="li"
          :to="item.path"
        >
          <a href="#">{{ item.name }}</a>
        </nuxt-link>
        <template v-if="isLogin">
          <li class="layui-hide-xs"><span class="line"></span></li>
          <li class="layui-hide-xs"><a href="">我发表的帖</a></li>
          <li class="layui-hide-xs"><a href="">我收藏的帖</a></li>
        </template>
      </ul>
      <div class="post layui-hide-xs">
        <span class="layui-icon layui-icon-search"></span>
        <router-link class="layui-btn" tag="button" :to="{ name: 'add' }"
          >发表新帖</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Panel',
  data() {
    return {
      navList: [
        {
          name: '首页',
          path: '/',
        },
        {
          name: '提问',
          path: '/home/ask',
        },
        {
          name: '分享',
          path: '/home/share',
        },
        {
          name: '讨论',
          path: '/home/discuss',
        },
        {
          name: '建议',
          path: '/home/advise',
        },
        {
          name: '公告',
          path: '/home/notice',
        },
        {
          name: '动态',
          path: '/home/logs',
        },
      ],
      isLogin: this.$store.state.isLogin,
    }
  },
}
</script>

<style lang="scss" scoped>
@media (max-width: 768px) {
  .panel {
    height: auto !important;

    ul {
      li {
        width: 33.33%;
        padding: 0 !important;
      }
    }
  }
}

.panel {
  height: 50px;
  line-height: 50px;
  background-color: #fff;
  position: relative;
  margin-bottom: 10px;

  ul {
    li {
      display: inline-block;
      padding: 0 20px;

      .line {
        display: inline-block;
        height: 15px;
        width: 2px;
        background-color: #e2e2e2;
        vertical-align: middle;
      }
      a:hover {
        color: #009688;
      }
    }
  }

  .post {
    position: absolute;
    right: 15px;
    top: 0;
    display: flex;
    align-items: center;

    span {
      margin-right: 10px;
      font-size: 24px;
      cursor: pointer;

      &:hover {
        color: #22dd92;
      }
    }
  }
}

.layui-this a {
  color: #009688;
}
</style>
