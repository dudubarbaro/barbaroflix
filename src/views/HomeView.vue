<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/8277479f">
        <img
          src="@/images/Cobra-Kai.jpg"
          alt="Cobra Kai Poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Cobra Kai</h3>
          <p>
            Anos depois de perder o grande torneio de caratê, Johnny Lawrence
            busca dar um novo significado para sua vida e abre o dojô Cobra Kai.
            Com isso, ele acaba reacendendo sua antiga rivalidade com Daniel
            LaRusso.
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input
        type="text"
        placeholder="O que você quer assistir hoje?"
        v-model="search"
      />
      <input type="submit" value="Procurar" />
    </form>
  </div>

  <div class="movie-list">
    <div class="movie" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster" />
          <div class="type">{{ movie.Type }}</div>
        </div>
        <div class="detail">
          <p class="year">{{ movie.Year }}</p>
          <h3>{{ movie.Title }}</h3>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            search.value = "";
          });
      }
    };

    return {
      search,
      movies,
      SearchMovies,
    };
  },
};
</script>
<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 600px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }
    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgb(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: white;
        margin-bottom: 16px;
      }

      p {
        color: white;
      }
    }
  }
  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        max-width: 500px;
        color: white;
        background-color: darkred;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: white;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgb(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 200px;
        background-color: white;
        padding: 16px;
        border-radius: 6px;
        color: black;
        font-size: 18px;
        text-transform: uppercase;
        transition: 0.1s;
        height: 50px;

        &:active {
          background-color: rgb(9, 8, 8);
          color: white;
        }
      }
    }
  }

  .movie-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: darkred;
            color: white;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }
        .detail {
          background-color: #496583;
          padding: 8px 16px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: white;
            font-size: 14px;
          }
          h3 {
            color: white;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
