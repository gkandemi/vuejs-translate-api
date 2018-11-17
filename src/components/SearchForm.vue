<template>
  <form @submit.prevent="translateIt" class="well">
    <textarea v-model="searchText" cols="30" rows="5" class="form-control" placeholder="Çevirmek istediğiniz kelime/cümle yazınız."></textarea>
    <select class="form-control" v-model="translateTo">
      <option v-for="(lang,key) in languages" :value="key"> {{ lang }}</option>
    </select>
    <br>
    <div class="text-left">
      <strong>Çevirilecek Dil : </strong> {{ languages[translateTo] }}
    </div>
    <br>
    <button type="submit" class="btn btn-primary btn-block">Çevir Gelsin!</button>
  </form>
</template>
<script>
  import axios from "axios"
  export default {
    data(){
      return {
        searchText : "",
        languages : {},
        translateTo : ""
      }
    },
    methods : {
      translateIt(){
        let url = "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20181116T200129Z.775558096056e445.a0cb98993d244e1a93e7aa435b7a0f60a6b5ab4d&text=" + this.searchText + "&lang=" + this.translateTo
        axios.get(url)
          .then(response => {

            let translatePack = {
              fromLang : this.languages[response.data.lang.split("-")[0]],
              toLang : this.languages[this.translateTo],
              fromText : this.searchText,
              toText : response.data.text[0]
            }
            this.$emit("translatedEvent", translatePack)
            this.searchText = ''
          }).catch(err => console.log(err))
      }
    },
    created(){
      axios.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20181116T200129Z.775558096056e445.a0cb98993d244e1a93e7aa435b7a0f60a6b5ab4d&ui=tr")
        .then(response => {
          this.languages = response.data.langs
        })
    }

  }
</script>
<style></style>
