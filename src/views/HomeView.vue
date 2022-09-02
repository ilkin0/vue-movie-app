<template>
  <div class="home">
    <!--    <div class="feature-card">-->
    <!--      <router-link to="/movie/asdfg">-->
    <!--        <img-->
    <!--          src="https://cdn.vox-cdn.com/thumbor/3LvEejiooUVeBfB-a4lNfiv0BNw=/0x0:2038x1012/920x613/filters:focal(791x298:1117x624):format(webp)/cdn.vox-cdn.com/uploads/chorus_image/image/53404039/getoutcover.0.jpg"-->
    <!--          alt="Get Out movie poster" class="featured-image">-->
    <!--        <div class="details">-->
    <!--          <h3>Get Out</h3>-->
    <!--          <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Explicabo, nobis, repudiandae! Consequuntur-->
    <!--            ducimus ea et eum illo in itaque, magnam, minus modi optio quas quos ratione sint unde velit,-->
    <!--            voluptatibus.</p>-->
    <!--        </div>-->
    <!--      </router-link>-->
    <!--    </div>-->

    <form @submit.prevent="searchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search">
      <!--      <input type="submit" value="Search">-->
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.id">
        <router-link :to="'/movie/' + movie.id">
          <div class="product-image">
            <img :src="imgPoster.concat(movie.poster_path)" alt="movie-poster">
            <div class="type">{{ movie.genres_ids }}</div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import env from '@/env.js'
import Swal from 'sweetalert2'

export default {
  setup () {
    const search = ref('')
    const movies = ref([])
    const imgPoster = env.IMAGE_POSTER_API

    const searchMovies = () => {
      if (search.value === '') {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Search must not be empty!',
          footer: '<a href="https://www.youtube.com/watch?v=FqOIlHomdSE">Why do I have this issue?</a>'
        })
        window.location.href = 'https://www.youtube.com/watch?v=FqOIlHomdSE'
        throw new Error('Search is empty')
      }

      // fetch(env.MOVIE_SEARCH_API
      //   .replace('appKey', env.appKey)
      //   .replace('searchValue', search.value).concat('&page=1'))
      //   .then(res => res.json())
      //   .then(data => {
      //     movies.value = data.results
      //     console.log(movies.value)
      //
      //     search.value = ''
      //   })
      fetch(env.TRENDING_API
        .replace(':apiKey', env.appKey).concat('&page=1'))
        .then(res => res.json())
        .then(data => {
          movies.value = data.results
          console.log(movies.value)

          search.value = ''
        })
    }

    return {
      search,
      movies,
      searchMovies,
      imgPoster
    }
  }
}
</script>

<style lang="scss">
.home {
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

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 16px;
    align-items: center;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type='text'] {
        width: 100%;
        color: #FFFFFF;
        background-color: #67686D;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #3A3F47;
        }

        &:focus {
          box-shadow: 0 3px 6px rgb(0, 0, 0, 0.2);
        }
      }
    }
  }
}
</style>
