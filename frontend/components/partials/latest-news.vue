<template>
  <div class="mnmd-block mnmd-block--fullwidth">
    <div class="container">
      <div class="row">
        <div class="mnmd-main-col" role="main">
          <div class="mnmd-block">
            <div class="block-heading block-heading--line-under">
              <h4 class="block-heading__title">
                <span class="first-word">Latest</span> News
              </h4>
            </div>
            <div class="posts-list list-space-lg">
              <PartialsCardNews
                v-for="{
                  id,
                  author,
                  title,
                  slug,
                  category,
                  poster,
                  tags,
                  body,
                  views,
                  created_at,
                } in (index, posts)"
                :key="id"
                :id="id"
                :author="author"
                :poster="poster"
                :title="title"
                :slug="slug"
                :tags="tags"
                :body="body"
                :category="category"
                :views="views"
                :created_at="created_at"
              />
            </div>
          </div>
          <!-- .mnmd-block -->
        </div>
        <!-- .mnmd-main-col -->
        <div
          class="mnmd-sub-col mnmd-sub-col--right js-sticky-sidebar"
          role="complementary"
        >
          <PartialsWidgetSpeedRead />

          <PartialsAdsBanner300x250 :key="useRoute().fullPath" />
          <!-- .widget -->
          <PartialsWidgetPopularPost />
          <!-- .widget -->
        </div>
        <!-- .mnmd-sub-col -->
      </div>
      <!-- .row -->
    </div>
    <!-- .container -->
  </div>
</template>

<script setup>
const posts = ref({});
try {
  const { data } = await useNuxtApp().$apiFetch("/graphql", {
    body: JSON.stringify({
      query: `
      query getLatestNews {
        posts(orderBy: [{ column: CREATED_AT, order: ASC }], first: 7) {
          data {
            id
            author {
              id
              username
            }
            title
            slug
            category {
              name
              slug
              parent_category {
                name
                slug
              }
            }
            poster
            tags
            body
            views
            created_at
            updated_at
          }
        }
      }`,
    }),
  });
  posts.value = data?.posts.data;
} catch (err) {
  console.log(err);
}
</script>

<style lang="scss" scoped>
</style>