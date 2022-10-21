<template>
  <div class="pokeList">
    <article v-for="(pokemon, index) in pokemons"
    :key="index"
    @click="setPokemonUrl(pokemon.url)">
      <img :src="imageUrl + pokemon.id + '.png'" width="100" height="100" alt="">
      <h3>{{ pokemon.name }}</h3>
      <p>{{ '#0'+index }}</p>
    </article>
    <div id="scroll-roll" ref="infinitescrolltrigger"/>
  </div>
</template>

<script>
  export default {
    props: [
      'imageUrl',
      'apiUrl'
    ],
    data: () => {
      return {
        pokemons: [],
        nextUrl: '',
        currentUrl: ''
      }
    },
    methods: {
      fetchData() {
        let apiUrl = new Request(this.currentUrl);
        fetch(apiUrl)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.nextUrl = data.next;
            data.results.map(pokemon => {
              pokemon.id = pokemon.url.split('/')
                .filter(function(part) { return !!part }).pop();
              this.pokemons.push(pokemon);
            });
          })
          .catch((error) => {
            console.log(error);
          })
      },
      scrollTrigger() {
        const observer = new IntersectionObserver((entries) => {
          entries.map(entry => {
            if(entry.intersectionRatio > 0 && this.nextUrl) {
              this.next();
            }
          });
        });

        observer.observe(this.$refs.infinitescrolltrigger);
      },
      next() {
        this.currentUrl = this.nextUrl;
        this.fetchData();
      },
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
      }
    },
    created() {
      this.currentUrl = this.apiUrl;
      this.fetchData();
    },
    mounted() {
      this.scrollTrigger();
    }
  }
</script>

<style lang="scss" scoped>
  .pokeList {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 10px;
    width: 100%;
    max-width: 510px;

    article {
      height: 190px;
      background-color: rgb(230, 230, 230);
      text-align: center;
      text-transform: capitalize;
      border-radius: 7px;
      cursor: pointer;
      h3 {
        margin: 0;
        color: rgb(53,70,84);
        font-weight: 500;
      }
      p {
        color: rgb(53,70,84);
      }
    }
  }

  #scroll-roll {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 150px;
  }
</style>
