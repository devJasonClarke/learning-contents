<template>
  <div>
    <nav>
      <ul>
        <li v-for="link of article.toc" :key="link.id">
          <NuxtLink
            :class="{ 'py-2': link.depth === 2, 'ml-7 pb-2': link.depth === 3 }"
            :to="`#${link.id}`"
            >{{ link.text }}</NuxtLink
          >
        </li>
      </ul>
    </nav>
    <div>
      {{ article.author.name }}
      {{ article.author.bio }}
    </div>
    <article>
      <nuxt-content :document="article" />

      <p>Post last updated: {{ formatDate(article.updatedAt) }}</p>
      <p>Title: {{ article.title }}</p>
    </article>
    <v-img
      src="https://source.unsplash.com/yImlavN5V90/"
      width="100%"
      height="600"
    />
    <div class="width">
      <NuxtLink
        v-if="prev"
        :to="{ name: 'blog-slug', params: { slug: prev.slug } }"
        class="font-bold text-primary hover:underline"
      >
       Prev: {{ prev.title }}
      </NuxtLink>
     
      <NuxtLink
        v-if="next"
        :to="{ name: 'blog-slug', params: { slug: next.slug } }"
        class="font-bold hover:underline"
      >
      Next:  {{ next.title }}
      </NuxtLink>
    
    </div>
    
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
   const article = await $content('articles', params.slug).fetch()

    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    return {
      article,
      prev,
      next
    }
  },

  methods: {
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("en", options);
    }
  }
};
</script>

<style lang="scss">

.width{
    min-width: 100%;
    display: flex;
    justify-content: space-between;
}
</style>
