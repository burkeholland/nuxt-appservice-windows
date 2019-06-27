<template>
  <div class="section">
    <h1 class="is-size-1">These Fake Photos</h1>
    <div class="columns is-multiline">
      <div
        v-for="photo in photos"
        :key="photo.id"
        class="column is-one-quarter"
      >
        <div class="card photo">
          <div class="card-image">
            <figure class="image">
              <img :src="photo.url" alt="photo.title" />
            </figure>
          </div>
          <div class="card-content">
            <div class="content">
              <p>{{ photo.title }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get(
      'https://jsonplaceholder.typicode.com/photos?_limit=100'
    )
    return { photos: data }
  }
}
</script>

<style>
.card.photo {
  height: 100%;
}
</style>
