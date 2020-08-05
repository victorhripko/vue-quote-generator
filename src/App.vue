<template>
  <div id="app">
    <div class="app-preloader" v-if="isLoading">
      <Preloader />
    </div>

    <div class="app-container" v-if="!authorQuotes">
      <button class="large-button"
              type="button"
              @click="getRandomQuot"
              :disabled="isLoading"
              v-if="quote"
      >
        <span class="large-button__name">
          Reload
        </span>
        <span class="lnr lnr-sync large-button__icon"></span>
      </button>
      <Blockquote v-if="quote" :text="quote.quoteText"/>
      <button v-if="quote" class="large-button" type="button" @click="author">
        <span class="large-button__name">
          {{quote.quoteAuthor}}
        </span>
        <span class="large-button__subname">
          {{quote.quoteGenre}}
        </span>
        <span class="lnr lnr-arrow-right large-button__icon"></span>
      </button>
    </div>

    <div class="app-container" v-else>
      <button class="large-button" type="button" @click="clearAuthor">
        <span class="large-button__name">
          {{authorQuotes.message}}
        </span>
        <span class="lnr lnr-arrow-left large-button__icon"></span>
      </button>
      <Blockquote v-for="item in authorQuotes.quotes" :key="item._id" :text="item.quoteText"/>
    </div>

    <div class="app-footer">
      <p class="app-footer__text">
        Victor Hripko @DevChallenges.io
      </p>
    </div>
  </div>
</template>

<script>
import Blockquote from './components/Blockquote/Blockquote.vue';
import Preloader from './components/Preloader/Preloader.vue';

const API = 'https://quote-garden.herokuapp.com/api/v2/quotes/random';
const API_AUTHOR = 'https://quote-garden.herokuapp.com/api/v2/authors/';

export default {
  components: {
    Blockquote,
    Preloader,
  },
  data() {
    return {
      quote: null,
      authorQuotes: null,
      isLoading: true,
    };
  },
  mounted() {
    this.getRandomQuot();
  },
  methods: {
    async getRandomQuot() {
      this.isLoading = true;
      try {
        const res = await fetch(API);
        const data = await res.json();
        this.quote = data.quote;
        this.isLoading = false;
      } catch (e) {
        console.error(e);
      }
    },
    async author() {
      this.isLoading = true;
      try {
        const res = await fetch(`${API_AUTHOR}${this.quote.quoteAuthor}`);
        const data = await res.json();
        this.authorQuotes = data;
        this.isLoading = false;
      } catch (e) {
        console.error(e);
      }
    },
    clearAuthor() {
      this.authorQuotes = null;
      this.getRandomQuot();
    },
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: white;
  color: #2c3e50;
}

#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.app-container {
  width: 100%;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.large-button {
  $root: &;

  position: relative;
  display: block;
  padding: 40px 70px 40px 30px;
  border: none;
  text-align: left;
  width: 100%;
  outline: none;
  cursor: pointer;
  background-color: transparent;
  transition: background-color 400ms cubic-bezier(0.075, 0.82, 0.165, 1);

  &__name,
  &__subname {
    display: block;
    width: 100%;
  }

  &__name {
    font-size: 18px;
    font-weight: 700;
    color: #4f4f4f;
    transition: color 400ms cubic-bezier(0.075, 0.82, 0.165, 1);

    @media screen and (min-width: 768px) {
      font-size: 24px;
    }
  }

  &__subname {
    font-weight: 500;
    font-size: 14px;
    color: #828282;
  }

  &__name + &__subname {
    margin-top: 8px;
  }

  &__icon {
    position: absolute;
    top: 50%;
    right: 30px;
    width: 38px;
    height: 38px;
    display: grid;
    place-items: center;
    transform: translateY(-50%);
    font-size: 20px;
    color: transparent;
    transition: color 400ms cubic-bezier(0.075, 0.82, 0.165, 1);
  }

  &:hover {
    background-color: #333333;

    #{$root}__name,
    #{$root}__icon {
      color: #f2f2f2;
    }
  }

  &:focus {
    box-shadow: inset 0 0 0 2px #333333;
  }
}

.app-preloader {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: grid;
  place-items: center;
  background-color: white;
  z-index: 100;
}

.app-footer {
  margin-top: auto;
  padding: 10px 20px;
  text-align: center;
  background-image: linear-gradient(#ddd,#ddd);
  background-size: 400px 1px;
  background-repeat: no-repeat;
  background-position: top;

  &__text {
    color: #ddd;
  }
}

* {
  box-sizing: border-box;
}
</style>
