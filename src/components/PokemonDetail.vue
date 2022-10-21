<template>
  <div class="detail">
    <div class="detail-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="">
      </div>
        <h2>{{ pokemon.name }}</h2>
        <p>{{ description }} </p>
      <div v-if="pokemon" class="data">
        <div class="details">
          <h3>Pokemon Details</h3>
          <div class="property">
            <div class="left">Base Experience</div>
            <div class="right">{{ pokemon.base_experience }} XP</div>
          </div>
          <div class="property">
            <div class="left">Height</div>
            <div class="right">{{ pokemon.height / 10 }} m</div>
          </div>
          <div class="property">
            <div class="left">Weight</div>
            <div class="right">{{ pokemon.weight / 10 }} kg</div>
          </div>
          <div class="property">
            <div class="left">Hp</div>
            <div class="right">{{ this.pokemon.stats[0].base_stat  }} pts</div>
          </div>
          <div class="property">
            <div class="left">Attack</div>
            <div class="right">{{ this.pokemon.stats[1].base_stat  }} pts</div>
          </div>
          <div class="property">
            <div class="left">Defense</div>
            <div class="right">{{ this.pokemon.stats[2].base_stat  }} pts</div>
          </div>
          <div class="property">
            <div class="left">Special Attack</div>
            <div class="right">{{ this.pokemon.stats[3].base_stat  }} pts</div>
          </div>
          <div class="property">
            <div class="left">Special Defense</div>
            <div class="right">{{ this.pokemon.stats[4].base_stat  }} pts</div>
          </div>
          <div class="property">
            <div class="left">Speed</div>
            <div class="right">{{ this.pokemon.stats[5].base_stat  }} pts</div>
          </div>
        </div>
        <div class="more-details">
          <h3>Pokemon Evolutions</h3>
          <div class="evolutions">
            <div class="evolution" 
              v-for="(name, index) in evolutions"
              :key="'name'+index">
              {{ name }}
            </div>
          </div>
          <h3>Pokemon Types</h3>
          <div class="types">
            <div class="type" 
              v-for="(value, index) in pokemon.types"
              :key="'value'+index">
              {{ value.type.name }}
            </div>
          </div>
          <h3>Abilities</h3>
          <div class="abilities">
            <div class="ability" 
              v-for="(value, index) in pokemon.abilities"
              :key="'value'+index">
              {{ value.ability.name }}
            </div>
          </div>
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
      <button class="close" @click="closeDetail">close</button>
    </div>
  </div>
</template>

<script>
  export default {
    props: [
      'pokemonUrl',
      'imageUrl'
    ],
    data: () => {
      return {
        show: false,
        pokemon: {},
        evolutions: [],
        description: "",
      }
    },
    methods: {
      fetchData() {
        let req = new Request(this.pokemonUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.pokemon = data;
            this.show = true;
            this.fetchEvolutions()
          })
          .catch((error) => {
            console.log(error);
          })
      },

      fetchEvolutions() {
        let req = new Request(this.pokemon.species.url);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.setDescription(data.flavor_text_entries[0].flavor_text)
            req = new Request(data.evolution_chain.url)
            fetch(req)
              .then((resp) => {
                if(resp.status === 200)
                  return resp.json();
              })
              .then((data) => {
                this.setEvolution(data.chain.species.name)
                data.chain.evolves_to.length == 0 ? undefined : this.setEvolution(data.chain.evolves_to[0].species.name)
                data.chain.evolves_to[0].evolves_to.length == 0 ? undefined : this.setEvolution(data.chain.evolves_to[0].evolves_to[0].species.name)
                data.chain.evolves_to[0].evolves_to[0].evolves_to.length == 0 ? undefined : this.setEvolution(data.chain.evolves_to[0].evolves_to[0].evolves_to[0].species.name)
              })
          })
      },

      setDescription( description ) {
        this.description = description
      },

      setEvolution( name ){
        this.evolutions.push( name )
      },

      closeDetail() {
        this.$emit('closeDetail');
      }
    },
    created() {
      this.fetchData();
    }
  }
</script>

<style lang="scss" scoped>
  .detail {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    position: fixed;
    top: 0;
    left: 0;
    padding: 90px 10px 10px;
    width: calc(100% - 20px);
    height: calc(100vh - 20px);
    background: rgba($color: #000000, $alpha: .7);

    .detail-view {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative;
      width: 100%;
      height: 80%;
      max-width: 50%;
      padding: 50px 0 0;
      background-color: #fff;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);

      @media screen and (max-width: 600px){
          max-width: 80%;
        }

      p {
        max-width: 50%;
        text-align: center;
      }
      .image {
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: -60px;
        width: 120px;
        height: 120px;
        background-color: #333;
        border-radius: 50%;
        overflow: hidden;
        box-shadow: 0 15px 30px rgba(0,0,0,.2),
                    0 10px 10px rgba(0,0,0,.2);
      }

      h2 {
        text-transform: capitalize;
      }
      .details {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 40%;
        margin-bottom: 40px;
        @media screen and (max-width: 600px){
          width: 70%;
        }
      }
      .more-details {
        @media screen and (max-width: 600px){
          margin-left: 20% ;
        }
      }

      .data{
        max-height: calc(100vh - 400px);
        overflow-y: auto;
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        justify-content: center;
        width: 100%;
        gap: 10%;

        .property {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          margin-bottom: 10px;

          .left { float: left; }
          .right { float: right; }
        }

        h3 {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          @media screen and (max-width: 600px){
          width: 60%;
        }
        }

        .types, .abilities, .evolutions {
          display: flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          width: 90%;
          max-width: 400px;

          .type, .ability, .evolution {
            margin: 0 10px 10px 0;
            padding: 5px 10px;
            border-radius: 20px;
            color: #fff;
            font-size: 1rem;
            letter-spacing: 2px;
            text-transform: capitalize;
            word-wrap: none;
            word-break: keep-all;
          }

          .type { background-color: #0A2E50; }
          .ability { background-color: #C73015; }
          .evolution { background-color: #5315c7; }
        }
      }

      .close {
        outline: none;
        border: none;
        border-radius: 5px;
        background-color: rgb(53,70,84);
        color: #efefef;
        padding: 10px 20px;
        margin-bottom: 20px;
        font-size: 1.2rem;
        margin-top: 5%;
        cursor: pointer;
      }
    }

    i {
      font-size: 2rem;
      color: #efefef;
    }
  }
</style>