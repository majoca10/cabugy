<template>
    <v-app id="inspire">
        <cabugy-header />
      <v-main class="grey lighten-3" style="padding:0px 0px 0px">
        <v-container>
          <v-row style="top: -11px; position: relative;">
            <v-col cols="12">
              <v-sheet rounded="lg">
                <v-list color="transparent">
                  <v-list-item
                    link
                    to="/infobasica"
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        Informacion Basica
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item
                    v-show="compra"
                    link
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        Compras
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-group
                  >
                    <template v-slot:activator>
                      <v-list-item-content>
                        <v-list-item-title>Vender</v-list-item-title>
                      </v-list-item-content>
                    </template>

                    <v-list-item
                      link
                      to="/artista"
                    >
                      <v-list-item-content>
                        <v-list-item-title>Artistas</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                    <v-list-item
                      link
                      to="/misproductos"
                    >
                      <v-list-item-content>
                        <v-list-item-title>Mis productos</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </v-list-group>
                  <v-list-item
                    link
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        Disputas
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item
                    link
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        Validaciones
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script>
    export default {
      auth : true,
      async mounted () {
      if(localStorage.getItem('auth._token.local') == 'false'){
        console.log(typeof(localStorage.getItem('auth._token.local')))
      }else{
        let response = await this.$axios.get('users/me')
        if(response && response.data){
            let me = response.data
            this.avatar = me.nombres.charAt(0).toUpperCase()
            this.nombres = me.nombres
            this.apellidos = me.apellidos
            this.email = me.email
            this.celular = me.celular
            this.perfil = me.imgperfil.url
            this.portada = me.imgportada.url
            this.ratinc = 4.4
            this.ratinv = 5
            this.confirmed = me.confirmed
            this.sms = me.sms
            this.biometrica = me.biometrica
            this.compra = me.compra
            this.vende = me.vende
        }
        console.log(this.perfil)
      }
    },
      data: () => ({
        avatar: '',
        nombres: '',
        apellidos: '',
        email: '',
        celular: '',
        perfil:'',
        portada:'',
        verimgperfil: false,
        veravatar: false,
        imgperfil: '',
        imgportada:'',
        compra: true,
        vende: false,
        sms: false,
        biometrica: false,
        confirmed: true,
        ratinc: 0,
        ratinv: 0,
        compra: true,
        vende: false,
        admins: [
        ['Management', 'mdi-account-multiple-outline'],
        ['Settings', 'mdi-cog-outline'],
      ],

      }),
    methods: {

        go : (route)=>{
                    window.location.href = route
                },
    }
    }
  </script>