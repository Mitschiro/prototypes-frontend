<template>
<v-app>
    <v-layout column justify-center align-center>
      <v-navigation-drawer permanent>
        <v-toolbar flat>
          <v-list>
            <v-list-tile>
              <v-list-tile-title class="title">
                Admin
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
            <v-list-tile-content>
              <v-list-tile-title>{{ item.title }}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
      </v-navigation-drawer>
    </v-layout>
  </v-app>
</template>

<script>

export default {
  components: {
    items: [
        {
          title: "Create Product",
          to: "/admin/create/product"
        }
    ]
  }
}
</script>
