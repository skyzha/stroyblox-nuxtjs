<template>
  <section id="posts">
    <PostPreview
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :excerpt="post.previewText"
      :thumbnailImage="post.thumbnailUrl"
      :id="post.id"
    />
  </section>
</template>

<script>
import PostPreview from '@/components/Blog/PostPreview'

export default {
  components: {
    PostPreview
  },
  // data() {
  //   return {
  //     posts: [
  //       {
  //         title: 'A New Beginning',
  //         previewText: "This will be awesome, dont't miss it!",
  //         thumbnailUrl:
  //           'https://images.unsplash.com/photo-1573400780054-c260eb4865b2?ixlib=rb-1.2.1&auto=format&fit=crop&w=401&q=80',
  //         id: 'a-new-beginning'
  //       },
  //       {
  //         title: 'A Second Beginning',
  //         previewText: "This will be awesome, dont't miss it!",
  //         thumbnailUrl:
  //           'https://images.unsplash.com/photo-1569358731637-7a6c762adf28?ixlib=rb-1.2.1&auto=format&fit=crop&w=334&q=80',
  //         id: 'a-second-beginning'
  //       }
  //     ]
  //   }
  // }
  asyncData(context) {
    return context.app.$storyapi
      .get('cdn/stories', {
        version: 'draft',
        starts_with: 'blog/'
      })
      .then((res) => {
        return {
          posts: res.data.stories.map((bp) => {
            return {
              id: bp.slug,
              title: bp.content.title,
              previewText: bp.content.summary,
              thumbnailUrl: bp.content.thumbnail
            }
          })
        }
      })
  }
}
</script>

<style scoped>
#posts {
  padding-top: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

@media (min-width: 35rem) {
  #posts {
    flex-direction: row;
  }
}
</style>
