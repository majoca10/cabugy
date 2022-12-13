<template>
  <v-app id="inspire">
    <cabugy-header />
      <v-main class="grey lighten-3">
        <v-container>
          <v-row>
              <cabugy-menu-boardusuario />
            <v-col>
              <v-sheet
                min-height="70vh"
                rounded="lg"
              >
              <v-container>
                    <cabugy-misproductos-artista />
              </v-container>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    <cabugy-footer />  
  </v-app>
</template>
  
  <script>

    export default {
      auth: true,
      data: () => ({
      }),
    }
  </script>