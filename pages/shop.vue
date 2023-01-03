<template>
  <v-app id="inspire">
    <cabugy-header />
      <v-main class="grey lighten-3">
        <v-container>
          <v-row>
            <cabugy-menu-index />
              <v-col>
                <v-sheet
                  min-height="70vh"
                  rounded="lg"
                >
                <v-sheet
                  min-height="70vh"
                  rounded="lg"
                >
                  <v-container>
                    <v-alert v-show= this.nopro type="error">
                    No hay productos relacionados a este artista.
                    </v-alert>
                      <v-row 
                      >
                      <template>
                      <div class="ma-2">
                        <v-container>
                          <v-row>
                            <v-col sm="4" offset-sm="1" md="12" offset-md="1">
                              <h1>Productos</h1>
                            </v-col>
                          </v-row>
                          <v-row>
                                <v-col sm="6" lg="3" align="center"
                                v-for="product in allproducts" :key="product.id">
                                <productcart :id="product.id" :name="product.artistas[0].nombre" :price="product.total" :image="product.artistas[0].imgartista.url"/>
                            </v-col>
                          </v-row>
                        </v-container>
                      </div>
                    </template>
                      </v-row>
                  </v-container>
                </v-sheet>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    <cabugy-footer />  
  </v-app>
</template>
<script>
import {mapGetters,mapActions} from 'vuex';
  export default {

    auth: false,

    computed: mapGetters(["allproducts"]),

    data: () => ({
      links: [
        'Dashboard',
        'Messages',
        'Profile',
        'Updates',
      ],
      model:'',
      artistas:[],
      indexpro: [],
      pro : false,
      nopro: false
    }),

    methods:{
      ...mapActions(["fetchdata"]),

    },

    created(){
      this.fetchdata();
  }

  }
</script>