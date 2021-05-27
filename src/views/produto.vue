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
                    <v-img width="450" height="670" :src="item.Poster"></v-img>
              </v-card-text>

              <v-card-actions>
                <v-btn elevation="15" outlined color="#29B6F6" text @click="favorito(item)">
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
#checklist {
  --background: #ffffff;
  --text: #414856;
  --check: #4F29F0;
  --disabled: #C3C8DE;
  --width: 100px;
  --height: 140px;
  --border-radius: 10px;
  background: var(--background);
  width: var(--width);
  height: var(--height);
  border-radius: var(--border-radius);
  position: relative;
  box-shadow: 0 10px 30px rgba(#414856, 0.05);
  padding: 30px 45px;
  display: grid;
  grid-template-columns: 30px auto;
  align-items: center;
  label {
    color: var(--text);
    position: relative;
    cursor: pointer;
    display: grid;
    align-items: center;
    width: fit-content;
    transition: color .3s ease;
    &::before,
    &::after {
      content:"";
      position: absolute;
    }
    &::before {
      height: 2px;
      width: 8px;
      left: -27px;
      background: var(--check);
      border-radius: 2px;
      transition: background .3s ease; 
    }
    &:after {
      height: 4px;
      width: 4px;
      top: 8px;
      left: -25px;
      border-radius: 50%;
    }
  }
  input[type="checkbox"] {
    -webkit-appearance: none;
    -moz-appearance: none;
    position: relative;
    height: 15px;
    width: 15px;
    outline: none;
    border: 0;
    margin: 0 15px 0 0;
    cursor: pointer;
    background: var(--background);
    display: grid;
    align-items: center;
    &::before,
    &::after {
      content:"";
      position: absolute;
      height: 2px;
      top: auto;
      background: var(--check);
      border-radius: 2px;
    }
    &::before {
      width: 0px;
      right: 60%;
      transform-origin: right bottom;
    }
    &::after {
      width: 0px;
      left: 40%;
      transform-origin: left bottom;
    }
    &:checked {
      &::before {
        animation: check-01 .4s ease forwards;
      }
      &::after {
        animation: check-02 .4s ease forwards;
      }
      + label {
        color: var(--disabled);
        animation: move .3s ease .1s forwards;
        &::before {
          background: var(--disabled);
          animation: slice .4s ease forwards;
        }
        &::after {
          animation: firework .5s ease forwards .1s;
        }
      }
    }
  }
}

@keyframes move {
  50% {
    padding-left: 8px;
    padding-right: 0px;
  }
  100% {
    padding-right: 4px;
  }
}
@keyframes slice {
  60% {
    width: 100%;
    left: 4px;
  }
  100% {
    width: 100%;
    left: -2px;
    padding-left: 0;
  }
}
@keyframes check-01 {
  0% {
    width: 4px;
    top: auto;
    transform: rotate(0);
  }
  50% {
    width: 0px;
    top: auto;
    transform: rotate(0);
  }
  51% {
    width: 0px;
    top: 8px;
    transform: rotate(45deg);
  }
  100% {
    width: 5px;
    top: 8px;
    transform: rotate(45deg);
  }
}
@keyframes check-02 {
  0% {
    width: 4px;
    top: auto;
    transform: rotate(0);
  }
  50% {
    width: 0px;
    top: auto;
    transform: rotate(0);
  }
  51% {
    width: 0px;
    top: 8px;
    transform: rotate(-45deg);
  }
  100% {
    width: 10px;
    top: 8px;
    transform: rotate(-45deg);
  }
}
@keyframes firework {
  0% {
    opacity: 1;
    box-shadow: 0 0 0 -2px #4F29F0, 0 0 0 -2px #4F29F0, 0 0 0 -2px #4F29F0, 0 0 0 -2px #4F29F0, 0 0 0 -2px #4F29F0, 0 0 0 -2px #4F29F0;
  }
  30% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    box-shadow: 0 -15px 0 0px #4F29F0, 14px -8px 0 0px #4F29F0, 14px 8px 0 0px #4F29F0, 0 15px 0 0px #4F29F0, -14px 8px 0 0px #4F29F0, -14px -8px 0 0px #4F29F0;
  }
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
      axios.get("http://www.omdbapi.com/?s="+ encodeURIComponent(pesquisa) + "&page=1&apikey=47a567fc&" )
      .then(response => (this.info = response.data));
    },

  
  btnClick(id) {
      window.open("https://www.imdb.com/title/"+ id +"/");
    },
  beforeCreate: function(){
    document.body.className = 'Filmes'
  }
}
};
</script>
