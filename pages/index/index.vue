<template>
  <div>
    <div class="ui-panel">
      <div class="nav">
        <ul>
          <li :class="{ 'layui-this': !isEnd && !tag }" @click="search('all')">
            综合
          </li>
          <li><span class="line"></span></li>
          <li
            :class="{ 'layui-this': isEnd === '0' }"
            @click="search('isEnd', '0')"
          >
            未结
          </li>
          <li><span class="line"></span></li>
          <li
            :class="{ 'layui-this': isEnd === '1' }"
            @click="search('isEnd', '1')"
          >
            已结
          </li>
          <li><span class="line"></span></li>
          <li
            :class="{ 'layui-this': tag === '精华' }"
            @click="search('tag', '精华')"
          >
            精华
          </li>
        </ul>
        <ul>
          <li
            :class="{ 'layui-this': sort === 'created' }"
            @click="search('sort', 'created')"
          >
            按最新
          </li>
          <li><span class="line"></span></li>
          <li
            :class="{ 'layui-this': sort === 'answer' }"
            @click="search('sort', 'answer')"
          >
            按热议
          </li>
          <li><span class="line"></span></li>
          <li
            :class="{ 'layui-this': sort === 'fav' }"
            @click="search('sort', 'fav')"
          >
            按积分
          </li>
        </ul>
      </div>
      <vue-listitem
        :lists="lists"
        :isEnd="notMore"
        @loadMore="loadMore()"
      ></vue-listitem>
    </div>
  </div>
</template>

<script>
import qs from 'qs'

import ListItem from '~/components/contents/ListItem'

export default {
  name: 'List',
  components: {
    'vue-listitem': ListItem,
  },
  data() {
    return {
      isTop: '0',
      page: 0,
      limit: 20,
      catalog: '',
      sort: 'created',
      isEnd: '',
      tag: '',
      notMore: false,
      isLoad: false,
    }
  },
  async asyncData({ $axios }) {
    const options = {
      isTop: '0',
      page: 0,
      limit: 20,
      catalog: '',
      sort: 'created',
      isEnd: '',
      tag: '',
    }
    const listResult = await $axios.$get(
      '/public/list?' + qs.stringify(options)
    )
    return {
      lists: listResult.data,
    }
  },
  methods: {
    async _getList() {
      this.isLoad = true
      const options = {
        isTop: this.isTop,
        page: this.page,
        limit: this.limit,
        catalog: this.catalog,
        sort: this.sort,
        isEnd: this.isEnd,
        tag: this.tag,
      }
      const res = await this.$axios.$get(
        '/public/list?' + qs.stringify(options)
      )
      if (!this.lists.length) {
        this.lists = res.data
      } else {
        this.lists = this.lists.concat(res.data)
        if (res.data.length < this.limit) {
          this.notMore = true
        }
      }
    },
    loadMore() {
      if (this.isLoad) return
      this.page++
      this._getList()
    },
    search(flag, val) {
      if (this.sort === val) return
      if (this.isEnd === val) {
        this.isEnd = ''
        this.lists = []
        this.page = 0
        this._getList()
        return
      }
      if (this.tag === val) {
        this.tag = ''
        this.lists = []
        this.page = 0
        this._getList()
        return
      }
      switch (flag) {
        case 'all':
          this.isEnd = ''
          this.tag = ''
          break
        case 'isEnd':
          this.isEnd = val
          break
        case 'tag':
          this.tag = val
          break
        case 'sort':
          this.sort = val
      }
      this.lists = []
      this.page = 0
      this._getList()
    },
    init() {
      this.lists = []
      this.notMore = false
    },
  },
}
</script>

<style lang="scss" scoped>
.nav {
  border-bottom: 1px solid #f2f2f2;
  display: flex;
  justify-content: space-between;
  ul {
    line-height: 40px;
    display: flex;
    li {
      cursor: pointer;
      padding: 0;
      height: 40px;

      .line {
        display: inline-block;
        width: 2px;
        height: 10px;
        background-color: #ddd;
        margin: 0 20px 0 10px;
        vertical-align: middle;
      }
      &.layui-this {
        color: #009688;
      }
    }
  }

  p {
    float: right;
    line-height: 40px;
    font-size: 12px;
    color: #999;
    span {
      color: #e6941a;
    }
  }
}
</style>
