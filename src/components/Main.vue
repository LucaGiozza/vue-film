<template>
<div class="main">
    
    <Ricerca @search="ricercaFilm" />

    <div>
        <p>Hover on flag to see the info</p>
        <h2>Film in lista :</h2>
        <ListaFilm v-for="film in filmsArray" :key="film.id" :info="film"/>
    </div>
    <div>
         <h2>Serie tv in lista:</h2> 
        <ListaFilm v-for="serieTv in tvArray" :key="serieTv.id" :info="serieTv"/>
        

    </div>
    <!-- <div>
        
        <ListaFilm v-for="serieTv in tvArray" :key="serieTv.id" :info="serieTv"/>
        

    </div> -->
   
   

    
    
    

</div>
  
</template>


<script>
import axios from 'axios';

import Ricerca from '@/components/Ricerca.vue';
import ListaFilm from '@/components/ListaFilm.vue';

export default {
    name: 'Main',
  
   
    components: {
       Ricerca,
       ListaFilm
      
  },
 
  data(){
      return {
          apiUrl:'https://api.themoviedb.org/3/search/movie',
          apiTvUrl:'https://api.themoviedb.org/3/search/tv',
           
          

          apiKey: 'cabe8922195f2ff0cfd386b87a764c5b',
          language:'it-IT',
        //    listaFilms : '',
        //    listaSeries:'',
          
           searchText :'',
        //    searchTextTv: '',
           filmsArray: '',
           tvArray: '',
           copertinaArray : ''
         
      }
      
  },
  computed:{
      filtroFilm(){
          return this.listaFilm.filter(element => {
             return element.name.includes(this.searchText.toLowerCase())
          });

          
      },

     



  },

  methods :{

      ricercaFilm(trovaFilm){
          this.searchText= trovaFilm;
          const request = {
               params:{
                      api_key: this.apiKey,
                      language: this.language,
                      query: trovaFilm,
                      
                   }

          };
          // chiamata con metodo all

          axios
           .all([
               axios.get(this.apiUrl, request),
               axios.get(this. apiTvUrl, request),
               
           ])

           .then(axios.spread((responseFilm, responseTv,) => {
               this.filmsArray= responseFilm.data.results;
               this.tvArray= responseTv.data.results;
               
            //    console.log(this.tvArray)


           }));

        //   prima chiamata
        //   axios
        //       .get(this.apiUrl,{
        //           params:{
        //               api_key: this.apiKey,
        //               query: trovaFilm,
                      
        //           }
        //       })
        //       .then(response => {
        //           console.log(response.data.results);
        //           this.filmsArray= response.data.results;
        //       });

        

        



            
          
          
      },

   
      
  }

}
</script>

<style lang="scss" scoped>
h2{
    color:red;
    width:300px;
    padding:22px;
    font-size:30px;
}
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    background-color: black;
}
p{
    color:red;
}

</style>