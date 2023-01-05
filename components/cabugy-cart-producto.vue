
<template>
  <v-col>
      <v-card class="box d-flex justify-space-around align-center ma-4">
        <v-img
          float-sm-left
          height="140"
          max-width="100"
          :src="'https://api.cabugy.com' + image"
        ></v-img>
        <h3 class="align-center pa-2 d-inline-block col-3 text-truncate">{{name}}</h3>
        <h3>${{price}}</h3>
      <div class="d-flex align-center">
        <span>
        <v-btn
        icon
        elevation="18"
        outlined
        small
        @click="$store.commit('decrease',item.id)"
        >-</v-btn>
        </span>
    
        <h3 class="mx-2">{{countity}}</h3>
    
        <span>
        <v-btn
        icon
        outlined
        small
        @click="$store.commit('increase',item.id)"
        >+</v-btn>
        </span>
      </div>
    
        <h3>${{total()}}</h3>
        <v-card-actions>
      <v-btn
        outlined
        rounded
        text
      >
        Quitar
      </v-btn>
    </v-card-actions>
      </v-card>
    </v-col>
</template>
<script>
export default {
  name:'cart-item',
  props:['image','name','price','countity','id'],
  data(){
    return{
      item:{
        id:this.id
      }
    }
  },
  methods:{
    total: function(){
      return (this.price*this.countity).toFixed(2)
    
      }
  }
}
</script>