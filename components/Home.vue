<template>
  <div class="theme-main__inner home">
    <div class="article-list">
      <div class="article-item" v-for="item in articleList">
        <div v-if="item.frontmatter.cover" class="article-cover">
          <router-link :to="item.path">
            <img :src="item.frontmatter.cover" alt="cover"/>
          </router-link>
        </div>
        <h3 class="article-title">
          <router-link :to="item.path">{{item.title}}</router-link>
        </h3>
        <div class="article-desc" v-html="item.excerpt"></div>
        <footer class="article-meta">
          <span><i class="icon-calendar"></i>{{formateDate(item.frontmatter.date)}}</span>
        </footer>
      </div>
    </div>
  </div>
</template>
<script>
import dayjs from 'dayjs'
import dayjsPluginUTC from 'dayjs/plugin/utc'

dayjs.extend(dayjsPluginUTC)
const DATE_MAP = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
export default {
  name: 'Home',
  computed: {
    // TODO 文章列表提取成公用
    articleList() {
      let list = this.$site.pages.filter(item => {
        return item.pid === 'post';
      });
      list = list.sort((a,b) => {
        let time1 = new Date(a.frontmatter.date);
        let time2 = new Date(b.frontmatter.date);
        return time2 - time1;
      })
      return list;
    }
  },
  methods: {
    formateDate(val) {
      return dayjs
        .utc(val)
        .format(this.$themeConfig.dateFormat)
    }
  }
}
</script>
<style lang="stylus">
.article-item
  display: block;
  overflow: hidden;
  margin-bottom: 1.75rem;
  border-radius: 6px;
  background var(--theme-card-background)
.article-title
  margin 0
  a
    display: block;
    padding: 1.5rem 1.5rem 0;
    transition: color .15s;
    font-size: 24px;
    line-height: 28px;
.article-desc
  padding: 0 1.5rem;
  opacity: .8;
  line-height: 24px;
  letter-spacing: 0.2px;
  a
    border-bottom: 1px dotted;
    transition: color .15s,border-color .15s,opacity .15s;
.article-meta
  margin: 1em 1.5rem 0;
  padding-bottom: 1.5rem;
  opacity: .63;
  [class^="icon-"]
    margin-right 0.4rem
</style>