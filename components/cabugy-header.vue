<template>
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="32"
        > C </v-avatar>

        <v-btn
          v-for="(link, idx) in links"
          :key="idx"
          text
          :to="`/${link.page}`"
        >
          {{ link.name }}
        </v-btn>
        <v-spacer></v-spacer>
        <v-btn
          v-if="!isAuthenticated"
          to="/login"
          text
          >Ingresar</v-btn
        >
        <v-btn
          v-if="!isAuthenticated"
          to="/registrar"
          text
          >Registrar</v-btn
        >
        <v-menu
          v-model="menu"
          :close-on-content-click="false"
          :nudge-width="200"
          offset-x
          v-show= menu
        >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="indigo"
          dark
          v-bind="attrs"
          v-on="on"
          text
          v-show= bmenu
        >
          Menu
        </v-btn>
      </template>

      <v-card>
        <v-list>
          <v-list-item>
            <v-list-item-avatar>
              <v-avatar color="red">
              <span class="white--text text-h5">{{ this.avatar }}</span>
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>{{ this.nombres + ' ' + this.apellidos }}</v-list-item-title>
              <v-list-item-subtitle>{{ this.email }}</v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-action>
            </v-list-item-action>
          </v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list-item-group
          v-model="selectedItem"
          color="primary"
        >
          <v-list-item to="/boardusuario">
            <v-list-item-icon>
              <v-icon> mdi-account-cog </v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title> Board </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item to="/rpasswordusuario">
            <v-list-item-icon>
              <v-icon> mdi-account-cog </v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title> Cambiar contraseña </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item @click="salir">
            <v-list-item-icon>
              <v-icon> mdi-arrow-left </v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title> Salir </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>

        <v-card-actions>
        </v-card-actions>
      </v-card>
    </v-menu>
      </v-container>
    </v-app-bar>
</template>
<script>
import { mapGetters } from "vuex";
  export default {
    auth : false,

    async mounted () {

      if(localStorage.getItem('auth._token.local') == 'false'){
        this.bmenu = false
      }else{
        try {
          let response = await this.$axios.get('users/me')
            if(response && response.data){
                let me = response.data
                this.bmenu = true
                this.avatar = me.nombres.charAt(0).toUpperCase()
                this.nombres = me.nombres
                this.apellidos = me.apellidos
                this.email = me.email
            }
      } catch (e) {
        if(e.response.data.message == "Invalid token." ){
          localStorage.setItem('auth._token.local', 'false') 
        }
      }

        
      }
    },

    data: () => ({
      links: [
        {name:'Cabugy', page:''},
        {name:'Acerca de Nosotros', page:'abaout'}
      ],
      fav: true,
      menu: false,
      message: false,
      hints: true,
      selectedItem: 0,
      items: [
        { text: 'Perfil', icon: 'mdi-account-cog' },
      ],
      avatar: '',
      nombres: '',
      apellidos: '',
      email: '',
      bmenu: false
    }),
    computed: {
        ...mapGetters(['isAuthenticated']),
        ...mapGetters(['loggedInUser']),
      },
     
      methods: {
        async salir() {
          await this.$auth.logout()
          this.menu = false
          this.bmenu = false
        }
  }
}
</script>