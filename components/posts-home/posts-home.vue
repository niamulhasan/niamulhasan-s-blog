<template>
  <div v-if="posts.length > 0" class="grid sm:grid-cols-1 md:grid-cols-2 justify-center gap-3">
    <div v-for="(post, index) in posts" :key="index" class="py-3 sm:max-w-xl sm:mx-auto md:mb-20">
      <div
        class="bg-gray-800 shadow-lg border-gray-700 max-h-80 border sm:rounded-3xl p-8 flex space-x-8 card-dark-fix"
      >
        <template v-if="postType === 'projects'">
          <div class="h-48 overflow-visible w-1/2">
            <img v-if="post.cover" :src="post.cover" class="rounded-3xl shadow-lg h-80 w-full object-cover" alt="" />
          </div>
        </template>
        <template v-else>
          <div class="h-48 overflow-visible w-1/2">
            <img class="rounded-3xl shadow-lg h-80 w-full bg-cover" src="https://picsum.photos/600/400" alt="" />
          </div>
        </template>

        <div class="flex flex-col w-1/2 space-y-4">
          <div class="flex justify-between items-start">
            <nuxt-link :to="`${postType}/${post.slug}`">
              <h2 class="text-3xl font-bold text-bn">{{ post.title }}</h2>
            </nuxt-link>
            <!-- <div class="bg-yellow-400 font-bold rounded-xl p-2">7.2</div> -->
          </div>
          <div>
            <div class="text-sm text-gray-400 text-bn">{{ post.category }}</div>
            <div class="text-lg text-gray-800">{{ formatDate(post.createdAt) }}</div>
          </div>
          <p class="text-gray-400 max-h-40 overflow-y-hidden text-bn">
            {{ post.description }}
          </p>
          <!-- <div class="flex text-2xl font-bold text-a">$83.90</div> -->
        </div>
      </div>
    </div>
  </div>

  <!-- <ul v-if="posts.length > 0" class="cards">
    <li
      v-for="(post, index) in posts"
      :key="index"
    >
      <nuxt-link
        :to="`${postType}/${post.slug}`"
        class="card card--clickable"
      >
        <template v-if="postType === 'projects'">
          <span class="flex-1">
            <h6 class="inline-block py-1 px-2 mr-1 bg-gray text-white text-sm font-medium rounded-sm">{{ post.category }}</h6>
            <h3 class="card-title">{{ post.title }}</h3>
            <p class="mt-2">{{ post.description }}</p>
          </span>
          <img
            v-if="post.cover"
            class="cover-image"
            :src="post.cover"
          >
        </template>

        <template v-else>
          <span class="w-full">
            <span class="flex justify-between align-baseline">
              <h3 class="card-title">{{ post.title }}</h3>
              <h6
                v-if="post.createdAt"
                class="self-start inline-block mt-0 py-1 px-2 bg-gray text-white text-base font-medium rounded-sm whitespace-no-wrap"
              >{{ formatDate(post.createdAt) }}</h6>
            </span>
            <p class="mt-2">{{ post.description }}</p>
          </span>
        </template>
      </nuxt-link>
    </li>
  </ul>
  <div v-else-if="loading" class="cards">
    <div v-for="placeholder in placeholderClasses" :key="placeholder.id" class="card">
      <content-placeholders :rounded="true" :class="placeholder">
        <content-placeholders-heading />
      </content-placeholders>
    </div>
  </div> -->
  <p v-else class="max-w-5xl mx-auto">
    {{ amount > 1 ? 'Posts not found' : 'Post not found' }}
  </p>
</template>

<style scoped>
.light .card-dark-fix {
  background: #fff;
  border-color: #f1f1f1;
}
</style>

<script>
export default {
  name: 'Posts',
  props: {
    postType: {
      type: String,
      default: 'blog',
      validator: (val) => ['blog', 'projects'].includes(val),
    },
    amount: {
      // ? https://content.nuxtjs.org/fetching#limitn
      type: Number,
      default: 10,
      validator: (val) => val >= 0 && val < 100,
    },
    sortBy: {
      // ? https://content.nuxtjs.org/fetching#sortbykey-direction
      type: Object,
      default: () => ({
        key: 'slug',
        direction: 'desc', // you probably want 'asc' here
      }),
      validator: (obj) => typeof obj.key === 'string' && typeof obj.direction === 'string',
    },
  },
  data() {
    return {
      posts: [],
      loading: true,
    }
  },
  computed: {
    placeholderClasses() {
      const classes = ['w-full', 'w-2/3', 'w-5/6']
      return [...Array.from({ length: this.amount }, (v, i) => classes[i % classes.length])] // repeats classes after one another
    },
  },
  async mounted() {
    this.loading = true
    this.posts = await this.fetchPosts()
    this.loading = false
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    async fetchPosts(postType = this.postType, amount = this.amount, sortBy = this.sortBy) {
      return this.$content(postType)
        .sortBy(sortBy.key, sortBy.direction)
        .limit(amount)
        .fetch()
        .catch((err) => console.error(err) || [])
    },
  },
}
</script>
