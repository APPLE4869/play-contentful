<template>
  <div>
    <!-- render data of the person -->
    <h1 v-if="person">{{ person.fields.name }}</h1>
    <!-- render blog posts -->
    <ul>
      <li v-for="(post, i) in posts" :key="i">
        <nuxt-link :to="`/${post.fields.slug}`">
          {{ post.fields.title }}
        </nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'

const client = createClient()

export default {
  data() {
    return {
      person: null,
      posts: []
    }
  },
  // `env` is available in the context object
  asyncData ({env}) {
    console.log("okok")
    return Promise.all([
      // fetch the owner of the blog
      // client.getEntries({
      //   'sys.id': env.CTF_PERSON_ID
      // }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt'
      })
    ]).then(([posts]) => {
      console.log(posts.items[0])
      return {
        // person: entries.items[0],
        posts: posts.items
      }
    }).catch(console.error)
  }
}
</script>

<style></style>
