<template>
  <div class="theme-main__inner post">
    <article
      itemscope
      itemtype="https://schema.org/BlogPosting"
    >
      <header v-if="$frontmatter.cover" :style="headerStyle" class="article__header--hasCover">
        <div class="article__header-con">
          <PostMeta
            class="post-meta--hasCover"
            :cates="$frontmatter.category || $frontmatter.categories"
            :author="$frontmatter.author"
            :date="$frontmatter.date"
            :location="$frontmatter.location"
          />
          <hr class="article-hr"/>
          <h1 class="post-title" itemprop="name headline">
            {{ $frontmatter.title }}
          </h1>
        </div>
      </header>
      <header v-else :style="headerStyle" class="article__header">
        <h1 class="post-title" itemprop="name headline">
            {{ $frontmatter.title }}
          </h1>
          <PostMeta
            :cates="$frontmatter.category || $frontmatter.categories"
            :author="$frontmatter.author"
            :date="$frontmatter.date"
            :location="$frontmatter.location"
          />
      </header>
      <div class="article-con">
        <Content class="article-content" itemprop="articleBody" />
        <Reward v-if="isShowReward"/>
      </div>
      <div class="article-footer">
        <PostTag v-if="$frontmatter.tags" :tags="$frontmatter.tags" />
        <PostNav/>
        <Comment/>
      </div>
    </article>
  </div>
</template>

<script>
import PostTag from '@theme/components/PostTag.vue'
import PostMeta from '@theme/components/PostMeta.vue'
import PostNav from '@theme/components/PostNav.vue'
import Reward from '@theme/components/Reward.vue'
import { Comment } from '@vuepress/plugin-blog/lib/client/components'
export default {
  name: 'Post',
  components: {
    PostTag,
    PostMeta,
    Comment,
    PostNav,
    Reward
  },
  computed: {
    isShowReward() {
      if (this.$frontmatter.reward === false) {
        return false;
      }
      return this.$themeConfig.reward.enable
    },
    headerStyle() {
      if (!this.$frontmatter.cover) return;
      return {
        'background-image': `url(${this.$frontmatter.cover})`,
        'background-color': this.$frontmatter.coverBgColor
      }
    },
    pageLink() {
      return `${this.$page.hostname}${this.$page.path}`;
    }
  }
}
</script>

<style lang="stylus">
.post
  background var(--theme-card-background)
  border-radius: 6px;
  line-height 1.8
  color var(--theme-foreground-color)
  h2, h3, h4, h5, h6
    margin: 2rem 0 1rem;
    font-weight: 700;
.article-content
  opacity: .8;
  a
    border-bottom: 1px dotted;
    transition: color .15s,border-color .15s,opacity .15s;
.article__header
  .post-title
    padding: 4.3rem 2.15rem 2.15rem;
    margin 0
    line-height: 32px;
  .post-meta
    padding: 0px 2.15rem;
    line-height: 3;
    background-color: var(--theme-accent-color-01);
.article__header--hasCover
  border-radius: 6px;
  padding-top: 37%;
  background-position: 50% center;
  background-size: cover;
  color: rgb(255, 255, 255);
.article__header-con
  padding: 2.5rem 2.15rem 1rem;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.75));
.article-copyright
  display: block;
  margin: 2rem 0;
  padding: 1rem 1.5rem;
  background-color: var(--theme-accent-color-005);
  border-left: 3px solid var(--theme-accent-color);
  border-radius: 3px;
  word-break: break-word;
  line-height: 24px;
  ul
    margin 0
    padding-left 0
  &__item
    display flex
    line-height 2
    align-items flex-start
    span
      margin: 0 .6em 0 .2em;
    p
      line-height 2
      margin 0
  &__title
    height 2em
    white-space nowrap
.article-con
  padding: 0.80625rem 2.15rem 2.15rem;
.article-footer
  padding: 0 2.15rem;
  border-color: var(--theme-border-color);
  .vssue
    padding: 10px 0;
.article-hr
  margin: .7rem 0 1rem;
  height: 2px;
  border: 0;
  background: #fff;
  opacity: .5;
  animation: .4s both;
  animation-name: line-scale;
.footnote-ref a
  &:link, &:visited
    color var(--theme-accent-color)!important
abbr
  cursor help
@media (max-width: $MQMobile)
  .post-title
    margin-top 0
</style>