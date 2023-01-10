<template>
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-col class="col-12 d-flex" style="position: static; height: 64px; width: 80%; margin: 0 auto;">
      <v-col class="col-3" style="height: 64px;padding: 2px; position: relative; top: -16px;" to="/">
        <img
        src="../assets/images/logo2.png"
        img-alt="Image"
        alt="user"
        height="55px"
        style= "cursor:pointer; margin: 0 auto; height: -webkit-fill-available; display: flow-root;"
        @click="toinicio"
        >
      </v-col>

        <v-col class="col-6">
        <v-btn
          v-for="(link, idx) in links"
          :key="idx"
          text
          :to="`/${link.page}`"
        >
          {{ link.name }}
        </v-btn>
        </v-col>
        <v-col class="col-3">
        <v-btn
          v-show= "this.noticart"
          text
          class="info"
          to="/cart"
        >
        <v-img style="max-width: 20px; margin-left: .5em" src="https://api.cabugy.com/uploads/cart_dd4562b8b4.svg" alt="Cart Icon"></v-img>
          Cart
          <span class="btn-circle">
            {{ this.cartlength }}
          </span>
        </v-btn>
        <v-btn
          v-show= "$store.state.cart.length >= 1  && this.noticart2"
          text
          class="info"
          to="/cart"
        >
        <v-img style="max-width: 20px; margin-left: .5em" src="https://api.cabugy.com/uploads/cart_dd4562b8b4.svg" alt="Cart Icon"></v-img>
          Cart
          <span class="btn-circle">
            {{ $store.state.cart.length }}
          </span>
        </v-btn>

        <v-btn
          v-if= "!isAuthenticated"
          to="/login"
          text
          >Ingresar</v-btn
        >
        <v-btn
          v-if= "!isAuthenticated"
          to="/registrar"
          text
          >Registrar
        </v-btn
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
    </v-col>
    </v-col>
    </v-app-bar>
</template>
<script>
  import {mapGetters} from 'vuex';
  export default {
    computed: mapGetters(["cartItemCount", "isAuthenticated"]),
    auth : false,

    async mounted () {

      if(localStorage.getItem('auth._token.local') == 'false'){
        this.bmenu = false
        this.noticart2 = true
        this.noticart = false
                        if(localStorage.getItem('product') != 'null'){
                  console.log('entra header if')
                  this.noticart2 = false
                  this.noticart = true
                  this.cartlength = 1
                }else{
                  console.log('entra header else')
                  window.localStorage.setItem('product', null);
                  this.noticart2 = true
                  this.noticart = false
                  this.cartlength = 0
                }
      }else{
        try {
          let response = await this.$axios.get('users/me')
            if(response && response.data){
                console.log(response.data)
                let me = response.data
                this.bmenu = true
                this.avatar = me.nombres.charAt(0).toUpperCase()
                this.nombres = me.nombres
                this.apellidos = me.apellidos
                this.email = me.email
                if(localStorage.getItem('product') != 'null'){
                  console.log('entra header if')
                  this.noticart2 = false
                  this.noticart = true
                  this.cartlength = 1
                }else{
                  console.log('entra header else')
                  window.localStorage.setItem('product', null);
                  this.noticart2 = true
                  this.noticart = false
                  this.cartlength = 0
                }
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
        {name:'¿Como funciona?', page:'abaout'},
        {name:'Carrito', page:'cart'}
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
      bmenu: false,
      cartlength: 0,
      noticart: false,
      noticart2: false
    }),
     
      methods: {
        async salir() {
          await this.$auth.logout()
          this.menu = false
          this.bmenu = false
        },

        toinicio(){
          this.$router.push('/')
        }
  }
}
</script>
<style>
  .btn-circle {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    position: absolute;
    top: -15px;
    right: -30px;
    background-color: #686868;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  body{
    font-family: Nunito Sans;
  }
</style>