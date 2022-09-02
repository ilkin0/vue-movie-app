<template>
  <div class="movie-detail">
    <!--    Details {{ $route.params.id }}-->
    <!--    <div class="feature-card" "getMovieById($route.params.id)">-->
    <div class="feature-card">
      <!--          <router-link to="/movie/asdfg">-->
      <img
        src="https://cdn.vox-cdn.com/thumbor/3LvEejiooUVeBfB-a4lNfiv0BNw=/0x0:2038x1012/920x613/filters:focal(791x298:1117x624):format(webp)/cdn.vox-cdn.com/uploads/chorus_image/image/53404039/getoutcover.0.jpg"
        alt="Get Out movie poster" class="featured-image">
      <div class="details">
        <!--        <h3>{{ getMovieById.title }}</h3>-->
        <h3>{{ movieById.title }}</h3>
        <p>{{ movieById.overview }}</p>
      </div>
      <!--          </router-link>-->
    </div>
  </div>
</template>

<script>
import env from '@/env.js'
import { onMounted } from 'vue'
import { useRoute } from 'vue-router'

export default {
  setup () {
    let movieById = {}
    const route = useRoute()

    onMounted(() => {
      const movieId = route.params.id
      fetch(env.MOVIE_GET_API.replace(':movieId', movieId)
        .replace(':appKey', env.appKey))
        .then(res => res.json())
        .then(data => {
          movieById = data
        })
      // .catch((data) => {
      //
      // })
    })

    return {
      movieById
    }
  }
}
</script>

<style lang="scss">
.movie-detail {
  .feature-card {
    position: relative;

    .featured-image {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }

    .details {
      position: absolute;
      right: 0;
      bottom: 0;
      left: 0;

      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #FFFFFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFFFFF;
      }
    }
  }
}
</style>
