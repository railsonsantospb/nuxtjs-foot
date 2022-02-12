<style scoped>
        @import '@/assets/css/screen.css';
</style>
<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="12">
      <v-card class="logo py-4 d-flex justify-center">
        <NuxtLogo />
        <VuetifyLogo />
      </v-card>

          <p>Vuetify is a progressive Material Design component framework for Vue.js. It was designed to empower developers to create amazing applications.</p>
          
          <template>
          <v-simple-table>
          <template v-slot:default>
            <thead id="dc">
              <tr>
                <th class="text-left">
                  Times
                </th>

                <th class="text-center" style=" width: 430px;">
                  Empate
                </th>
                
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in desserts"
                :key="item.id"
              >
                <td style="font-size: 10px">{{ item.name }}</br><p id="data1">{{ item.date }}</p></td>
                
               
                <td style="display: table-cell">
                  <v-btn small  v-bind:id="item.casa[1]"
                  v-on:click="idOpcao(item.casa[1], item.casa[1], item.empate[1], item.fora[1])">
                    {{ item.casa[0] }}
                   
                  </v-btn>
                  
                  <v-btn small style="width: 10px" v-bind:id="item.empate[1]"
                  v-on:click="idOpcao(item.empate[1], item.empate[1], item.casa[1], item.fora[1])">
                    {{ item.empate[0] }}
                  </v-btn>

                  <v-btn small style="width: 10px" v-bind:id="item.fora[1]"
                  v-on:click="idOpcao(item.fora[1], item.fora[1], item.empate[1], item.casa[1])">
                    {{ item.fora[0] }}
                  </v-btn>
                  
                </td>
                
              </tr>
            </tbody>
          </template>
        </v-simple-table>
        </template>
          <p>Thank you for developing with Vuetify and I look forward to bringing more exciting features in the future.</p>
          <div class="text-xs-right">
            <em><small>&mdash; John Leider</small></em>
          </div>
          <hr class="my-3">
          <a
            href="https://nuxtjs.org/"
            target="_blank"
            rel="noopener noreferrer"
          >
            Nuxt Documentation
          </a>
          <br>
          
    
       
     
    </v-col>
  </v-row>
</template>

<script>

import moment from 'moment';
import axios from 'axios';

export default {
  methods: {
        idOpcao(valid, id1, id2, id3){
          if(document.getElementById(valid).style.backgroundColor != "red"){
              document.getElementById(id1).style.backgroundColor = "";
              document.getElementById(id2).style.backgroundColor = "";
              document.getElementById(id3).style.backgroundColor = "";
              document.getElementById(valid).style.backgroundColor = "red";
           
          } else {
              document.getElementById(valid).style.backgroundColor = "";
          }
        }
  },
  name: 'IndexPage',
  data () {
    return {
      mountains: [],

      desserts: [],
    }
  },
  
  async fetch () {
    this.mountains = await axios.get(
      'https://www.arenabrasil.bet/web/principal/prematch?tipoEsporte=1', {

      }).then(res =>
      res.data).then((fixtures) => {
        
        fixtures.campeonatos.map((f) =>{
          f.momentos.map((m) =>{
            m.eventos.map((j) =>{

              let dates = moment(new Date(j.data)).format('DD/MM/YYYY hh:mm').split(' ')[0];
              if(dates){
                let idCasa = j.id+''+j.subeventos[0].aposta;
                let idEmpate = j.id+''+j.subeventos[1].aposta;
                let idFora = j.id+''+j.subeventos[2].aposta;
                this.desserts.push({name: j.casa + ' x ' + j.fora, date: dates + ' ' +
                j.horario, casa: [j.subeventos[0].cotacao/100, idCasa], 
                empate: [j.subeventos[1].cotacao/100, idEmpate],
                fora: [j.subeventos[2].cotacao/100, idFora] })

              }
            })
          })
          
        })
      });
  },


 

}

</script>
