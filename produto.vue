<template>
  <v-container>
      <input type="text" v-model="pesquisa" v-on:keyup.enter="procura(pesquisa)" >
          <button @click="procura(pesquisa)">Search</button>
            <p> Search your movie among our {{info.totalResults}} movies found</p>
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
        <v-card min-width="450" max-width="450" min-height="866" max-height="1000"
            class="mx-auto my-12  " v-for="(item, index) in info.Search" :key="index">
            
                <v-card-title class="film" >
                  Title:  {{item.Title}}  {{info.Title}}<br>
                  Year: {{item.Year}}  {{info.Year}}<br>
                  Type: {{item.Type}}  {{info.Type}}<br> 
                  <v-img width="300" :src="info.Poster"></v-img>                  
                  </v-card-title>
                
                <v-card-text>
                  <v-btn color="#FDD835" id="link" href="#"  v-on:click.native='btnClick(item.imdbID)' target="_blank"> IMDB PAGE </v-btn>
                </v-card-text>
                
                <v-card-text max-width="450" min-height="670" class="poster">
                    <v-img v-if="item.Poster != 'N/A'" width="450" height="670" :src="item.Poster"></v-img>
                    <v-img v-else width="450" height="670" :src="not_found"></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn elevation="15" outlined color="#29B6F6" text @click="favorito(item)">
                  Guardar como Favorito
                </v-btn>
              </v-card-actions>
          </v-card >
      </div>
  </div>
    <v-app v-if="keep_search != null || info.totalResults <= 10 " id="inspire">
        <div class="text-center">
        <v-container max-width="100" min-height="100">
            <v-row justify="center">
            <v-col>
                <v-container>
                <v-pagination v-model="page" class="my-1" :length="calculaPaginas(info.totalResults)" :total-visible="11" @input="carregaPagina"></v-pagination>
                </v-container>
            </v-col>
            </v-row>
        </v-container>
        </div>
    </v-app>
  </v-container>
  
</template>
<style>
.inspire{
  align-items: center;
  justify-items: center;
  grid-template-columns: auto auto auto;
}

.v-application--wrap{
  height: 130px;
}

.poster{
  display: flex;
  justify-content: center;
}

.row {
  display: block;
  margin: auto;
}


.colunaProduct {
  flex: 80%;
  padding: 10px;
  display: grid;
  gap: 4px;
  align-items: center;
  justify-items: center;
  grid-template-columns: auto auto auto;
  grid-row-gap: 100px;
}

.film {
  text-transform: capitalize;
  display: table;
  table-layout: fixed;
  width: 100%;
  flex-grow: 1;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.colunaFav {
  flex: 40%;
  padding: 10px;
}
</style>

<style lang="scss">
:root {
  --color: rebeccapurple;
  --disabled: #959495;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

.checkbox {
  display: grid;
  grid-template-columns: min-content auto;
  grid-gap: 0.5em;
  font-size: 2rem;
  color: var(--color);

  &--disabled {
    color: var(--disabled);
  }
}

.checkbox__control {
  display: inline-grid;
  width: 1em;
  height: 1em;
  border-radius: 0.25em;
  border: 0.1em solid currentColor;

  svg {
    transition: transform 0.1s ease-in 25ms;
    transform: scale(0);
    transform-origin: bottom left;
  }
}

.checkbox__input {
  display: grid;
  grid-template-areas: "checkbox";

  > * {
    grid-area: checkbox;
  }

  input {
    opacity: 0;
    width: 1em;
    height: 1em;

    &:focus + .checkbox__control {
      box-shadow: 0 0 0 0.05em #fff, 0 0 0.15em 0.1em currentColor;
    }

    &:checked + .checkbox__control svg {
      transform: scale(1);
    }

    &:disabled + .checkbox__control {
      color: var(--disabled);
    }
  }
}
</style>


<script>

import axios from "axios";

export default {
  data() {
    return {
      info: null,
      page: 1,
      favoritos: [],
      keep_search: null,
      not_found: "https://westsiderc.org/wp-content/uploads/2019/08/Image-Not-Available.png",
      watched: false,
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
      .then(response => (this.info = response.data));
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
      this.keep_search = pesquisa
      axios.get("http://www.omdbapi.com/?s="+ encodeURIComponent(pesquisa) + "&page=1&apikey=47a567fc&" )
      .then(response => (this.info = response.data));
    },
  
  btnClick(id) {
      window.open("https://www.imdb.com/title/"+ id +"/");
    },
  beforeCreate: function(){
    document.body.className = 'Filmes'
  },
  
  carregaPagina(page){
    axios
    .get("http://www.omdbapi.com/?s="+encodeURIComponent(this.keep_search)+"&page="+page+"&apikey=47a567fc&")
    .then(response => (this.info = response.data));
  },

  calculaPaginas(resultados){
    var resto_divisao = resultados % 10
    var paginas = (resultados / 10) - (resto_divisao / 10)
    return paginas
  },
}
};
</script>
