<template>
  <div v-if="post">
    <h1>{{ post.fields.title }}</h1>
    <p>{{ post.fields.body }}</p>
  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'

const client = createClient()

export default {
  data() {
    return {
      post: null
    }
  },
  `env` is available in the context object
  asyncData ({env, params}) {
    console.log(params.slug)
    return Promise.all([
      // fetch the owner of the blog
      // client.getEntries({
      //   'sys.id': env.CTF_PERSON_ID
      // }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        'content_type': env.CTF_BLOG_POST_TYPE_ID,
        'fields.slug': params.slug,
        order: '-sys.createdAt'
      })
    ]).then(([entries]) => {
      // return data that should be available
      // in the template
      // console.log(entries)
      console.log(entries.items)

      return {
        // person: entries.items[0],
        post: entries.items[0]
      }
    }).catch(console.error)
  }
}
</script>

<style></style>
