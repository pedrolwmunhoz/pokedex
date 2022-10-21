<template>
  <div class="searchbar">
    <form class="form" @submit.prevent="setPokemonUrl">
      <input type="text" placeholder="Search name" v-model="searchvalue">
      <button class="button" @click="setPokemonUrl">Search</button>
    </form>
  </div>
</template>

<script>
  export default {
    props: [
      'apiUrl'
    ],
    data: () => {
      return {
        searchvalue: '',
      }
    },
    methods: {
      setPokemonUrl() {
        if(this.searchvalue !== '')
          this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue);
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;500;700&display=swap');

  .searchbar {
    position: relative;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;

    input {
      border: solid 1px rgba(97, 97, 97, 0.2);
      outline: none;
      border-radius: 5px;
      padding: 10px 40px 10px 10px;
      width: calc(100% - 50px);
      font-size: 1rem;
      box-shadow: 0 15px 30px rgba(0,0,0,.2)
    }
    form {
      display: flex;
      flex-direction: row;
      gap: 4px;
    }
    button {
      font-family: 'Roboto', sans-serif;
      font-weight: 400;
      width: 150px;
      border-radius: 7px;
      border: none;
      background-color: rgb(53,70,84);
      color: white;
      cursor: pointer;
    }
  }
</style>