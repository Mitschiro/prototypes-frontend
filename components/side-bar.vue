<template>
    <v-app dark>
    <v-layout column align-left>
      <v-navigation-drawer permanent dark app>
        <v-toolbar flat>
          <v-list>
            <v-list-tile>
              <v-list-tile-title class="title">

              </v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-toolbar>
    
        <v-divider></v-divider>
    
        <v-list dense class="pt-0">
          <v-list-tile
            v-for="item in items"
            :key="item.title"
          >
           <v-list-tile-content >
            <nuxt-link :to="item.to">{{ item.title }}</nuxt-link>
          </v-list-tile-content>
          </v-list-tile>
         
        </v-list>
      </v-navigation-drawer>
    </v-layout>
  </v-app>
</template>

<script>
export default {
    data: () => {
        return {
        items: [
            {
              title: "Admin",
              to: "/admin"
            },
            {
              title: "Wizard",
              to: "/wizard"
            },
            {
              title: "Rules",
              to: "/rules"
            },
            {
              title: "Items Preview",
              to: "/items"
            }
        ]
        }
    }
}
</script>

<style>

</style>
