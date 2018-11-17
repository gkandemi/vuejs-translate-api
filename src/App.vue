<template>
  <div class="container">
    <h1 class="text-center">Vue-Translator</h1>
    <h5 class="text-center">Vue.js ile Translator</h5>
    <br>
    <div class="row">
      <div class="col-md-12 text-center">
        <SearchForm @translatedEvent="addHistory($event)" />
        <h3 class="text-success text-center"> {{ translatedText }} </h3>
        <History :history="history" v-if="history.length > 0" />
      </div>
    </div>
  </div>
</template>
<script>
  import History from "./components/History"
  import SearchForm from "./components/SearchForm"

  export default {
    components : {
      History,
      SearchForm
    },
    data(){
      return {
        translatedText : "",
        history : []
      }
    },
    methods : {
      addHistory(translatePack){
        this.translatedText = translatePack.toText
        this.history.push(translatePack)
        console.log(this.history)
      }
    }
  }
</script>

<style>
  body{
    background-color: #dedede;
  }
</style>
