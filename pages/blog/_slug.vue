<template>
  <article>
    <author :author="article.author"></author>

    <h1>{{ article.title }}</h1>
    <p>{{ article.description }}</p>
    <img :src="article.image" :alt="article.alt" />
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

    <nuxt-content :document="article" />

    <nav>
      <ul>
        <li v-for="link of article.toc" :key="link.id">
          <NuxtLink
            :to="`#${link.id}`"
            :class="{ 'py-2': link.depth === 2, 'ml-2 pb-2': link.depth === 3 }"
            >{{ link.text }}</NuxtLink
          >
        </li>
      </ul>
    </nav>
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()

    return { article }
  },
  methods: {
    formatDate(date) {
      const options = {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: 'numeric',
        minute: 'numeric',
      }
      return new Date(date).toLocaleString('en', options)
    },
  },
}
</script>

<style>
.nuxt-content h2 {
  @apply bg-red-500 font-bold text-2xl;
}
.nuxt-content h3 {
  @apply font-bold text-xl;
}
.nuxt-content p {
  @apply mb-2;
}
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
</style>
