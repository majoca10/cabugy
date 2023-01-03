<template>
    <v-app>
        <cabugy-header />
        <v-col>
        <v-sheet
            min-height="70vh"
            rounded="lg"
        >
        <div v-if="cartItemCount>0">
        <h1 class="text-center d-block mt-16">Carrito de compra</h1>
        <v-row class="d-flex justify-center">
            <v-col cols="12" lg="8" sm="12">
                <div class="d-flex justify-center">
                    <div class="mt-14">
                        <div class="d-flex flex-column item-box mt-4">
                            <div class="d-flex mb-6">
                                <h3 class="mr-auto pa-2">Product</h3>
                                <h3>Precio</h3>
                                <h3>Cantidad</h3>
                                <h3>Total</h3>
                                <h3>Accion</h3>
                            </div>
                            <cabugy-cart-producto v-for="item in $store.state.cart" :key="item.id"
                            :name="item.artistas[0].nombre" :image="item.artistas[0].imgartista.url" :price=parseInt(item.total) 
                            :countity="item.cantidad" :id="item.id"/>
                            </div>
                    </div>
                </div>
            </v-col>
        <v-col sm="6" lg="3">
            <div class="d-flex box align-center justify-center">
                <v-card
                dark
                min-width="350"
                >
                <v-card-title>
    
                <h2 class="text-h6 font-weight-light ml-4">Ordenes</h2>
                </v-card-title>
                <hr class="mx-12">
    
                <v-card-text class="text-h6">
                <h4><span>Subtotal:</span><span>${{decimal(cartTotalPrice)}}</span></h4>
                </v-card-text>
                
                <v-card-text class="text-h6">
                <h4><span>Envio:</span><span>${{shipping=15000}}</span></h4>
                </v-card-text>
                <hr class="mx-12">
    
                <v-card-text class="text-h6">
                <h4><span>Total:</span><span>${{decimal(cartTotalPrice+shipping)}}</span></h4>
                </v-card-text>
                <v-card-actions>
                <v-btn
                color="primary"
                class="mt-2"
                height="50"
                block
                outlined
                >PAGAR</v-btn>
                </v-card-actions>
                </v-card>
            </div>
    
        </v-col>
        </v-row>
        </div>
        <div v-else class="d-flex mt-16 justify-center">
            <h1>No hay productos en el carrito</h1>
        </div>
    </v-sheet>
</v-col>
    <cabugy-footer />  
    </v-app>
    </template>
    
    <script>
    import {mapGetters} from 'vuex';
    export default {
    auth : false,
    
        computed: mapGetters(["cartTotalPrice","cartItemCount","allproducts"]),
        methods:{
          decimal: function(price){
            console.log(price)
            return (parseInt(price)).toFixed(2)
          },
         
        },
    }
    
    </script>
    <style scoped>
    .item-box{
      max-width: 850PX;
      flex-shrink: 1;
    }
    h3{
      margin: 0 40px;
      line-height: auto;
      text-align: center;
      width: 80px;
    }
    hr{
      border:0.5px;
      height: 1px;
      background-color: #333333;
    }
    .box{
        margin-top: 8px;
    }
    h4{
        display: flex;
        justify-content: space-between;
        margin:0 20px;
    }
    </style>