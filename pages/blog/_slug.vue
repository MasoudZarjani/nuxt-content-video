<template>
  <article class="container mx-auto my-10">
    <header class="text-4xl text-center font-bold">
      {{ article.title }}
      <p class="text-xl">
        {{ article.createdAt | formatDate }}
      </p>
    </header>
    <nuxt-content :document="article" class="w-3/4 mx-auto py-6" />
    <nuxt-link to="/blog" class="text-xl ml-64 font-bold">
      🔙 Go Back
    </nuxt-link>
  </article>
</template>

<script>
import { format } from 'date-fns'
export default {
  filters: {
    formatDate () {
      return format(new Date(), 'dd MMM yyyy')
    }
  },
  async asyncData ({ $content, app, params }) {
    const { slug } = params
    let article
    try {
      article = await $content(`${app.i18n.locale}/blog`, slug).fetch()
    } catch (error) {
      try {
        article = await $content(
          `${app.i18n.defaultLocale}/blog`,
          slug
        ).fetch()
      } catch (error) {
        return error({ statusCode: 404, message: 'Page not found' })
      }
    }
    return {
      article
    }
  }
}
</script>

<style>
.nuxt-content {
  width: 700px;
}
.nuxt-content h2 {
  font-weight: bold;
  font-size: 20px;
  margin-top: 15px;
}
.nuxt-content p {
  margin-top: 5px;
}
/* .icon.icon-link {
  background-image: url("~assets/hashtag.svg");
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
} */
</style>
