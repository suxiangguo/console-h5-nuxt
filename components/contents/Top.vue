<template>
  <div>
    <div class="ui-panel">
      <h3 class="title">置顶</h3>
      <vue-listitem
        :lists="lists"
        :is-end="isEnd_"
        @loadMore="loadMore()"
      ></vue-listitem>
    </div>
  </div>
</template>

<script>
import ListItem from './ListItem'

export default {
  name: 'Top',
  components: {
    'vue-listitem': ListItem,
  },
  props: {
    lists: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isEnd_: false,
      isLoad: false,
    }
  },
  methods: {
    async _getList() {
      this.isLoad = true
      const res = await this.$axios.$get('/public/topList')
      this.isLoad = false
      if (res.code === 10000) {
        this.lists = res.data
      } else {
        this.$pop(res.message)
      }
    },
    loadMore() {
      if (this.isLoad) return
      this.page++
      this._getList()
    },
  },
}
</script>

<style lang="scss" scoped>
li {
  overflow: hidden;
  padding: 10px 0;
  height: 60px;
  border-bottom: 1px solid #f2f2f2;
  .pic {
    width: 60px;
    height: 60px;
    background-color: #ddd;
    float: left;
    margin-right: 10px;
  }

  .content {
    display: flex;
    justify-content: space-between;
    .info {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 60px;
      .news-title {
        color: #333;
        font-size: 16px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        width: 100%;
        span {
          display: inline-block;
          border: 1px solid #2bd5b3;
          font-size: 12px;
          padding: 1px 2px;
          border-radius: 4px;
          color: #2bd5b3;
          margin-right: 4px;
        }
      }

      .tags {
        font-size: 14px;
        color: #999;
        display: flex;
        align-items: center;
        justify-content: space-around;
        width: 300px;

        .user {
          display: flex;
          align-items: center;
          span {
            display: inline-block;
            font-size: 12px;
            padding: 1px 2px;
            border-radius: 4px;
            color: #2bd5b3;
            margin-right: 4px;
            background-color: #dd4822;
            color: #fff;
            margin-left: 4px;
          }

          svg {
            width: 20px;
            height: 20px;
            margin-right: 2px;
          }
        }

        .favs {
          color: #f76809;
          display: flex;
          align-items: center;
          justify-content: center;
          svg {
            width: 16px;
            height: 16px;
            margin-right: 2px;
          }
        }

        .end {
          background-color: #4db38a;
          padding: 3px 3px;
          color: #fff;
          font-size: 12px;
          border-radius: 4px;
        }
      }
    }
    .count {
      display: flex;
      align-items: flex-end;

      span {
        margin-right: 6px;
      }
    }
  }
}
</style>
