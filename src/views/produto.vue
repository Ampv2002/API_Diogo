<template>
    <v-container>
      <input type="text" v-model="pesquisa" v-on:keyup.enter="procura(pesquisa)" >
          <button @click="procura(pesquisa)">Search</button>
          <hr/>
        <div class="row">
         
        <!-- Lista favoritos -->
        <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>

            <div class="colunaFav">

                <div v-if="favoritos.length >0">
                  <h3>Favoritos</h3>
                  <div v-for="(fav, index) in favoritos" :key="index">
                      {{fav.Title}} <v-icon x-small @click="removeFav(index)">mdi-close-circle-outline</v-icon>
                  </div>
                </div>

          </div>
<!-- Lista Produtos -->
    
      <div class="colunaProduct">
        <v-card max-width="450"
            class="mx-auto my-12  " v-for="(item, index) in info" :key="index">
                <v-card-title class="film">
                  Titulo:  {{item.Title}} <br>
                  Year: {{item.Year}} <br>
                  Type: {{item.Type}}
                   
                  </v-card-title>
                <v-card-text max-width="415" class="poster"   >
                    <v-img width="300" :src="item.Poster"></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn color="deep-purple " text @click="favorito(item)">
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
        </div>
  </div>
   </v-container>
</template>

<style>
.poster{
  display: flex;
  justify-content: center;
}
.film {
  text-transform: capitalize;
}
.row {
  display: flex;
}

.colunaProduct {
  flex: 60%;
  padding: 10px;
}

.colunaFav {
  flex: 40%;
  padding: 10px;
}
</style>



<script>
import axios from "axios";

export default {
  data() {
    return {
      info: null,
      favoritos: [],
      vertical: true,
      multiLine: true,
      snackbar: false,
      text: `Esse item já foi adicionado aos favoritos`,
    };
  },
  mounted() {
    var that = this;
    axios
      .get("http://www.omdbapi.com/?s=Batman&page=1&apikey=47a567fc&")
      .then(response => (this.info = response.data.Search));
    console.log(that.info);
  },
  methods: {
    favorito(item) {
      if (this.favoritos.indexOf(item ) === -1){
        this.favoritos.push(item);
        
      }else {
        this.snackbar = true;
      }
      console.log(this.favoritos);
    },

    removeFav(item){
      this.favoritos.splice(item, 1)

    },

    procura(pesquisa) {
      axios.get("http://www.omdbapi.com/?s="+ encodeURIComponent(pesquisa) + "&apikey=47a567fc&" )
      .then(response => (this.info = response.data.Search));
    }
  },
};
</script>
