<template>
  <v-app>
    <v-sheet style="height: 10vh">
      <v-container fluid><h1>Header</h1></v-container>
    </v-sheet>  
     <v-card>
      <v-toolbar
        color="cyan"
        dark
        flat
      >
        <v-app-bar-nav-icon></v-app-bar-nav-icon>
  
        <v-toolbar-title>GICT Test Navbar</v-toolbar-title>
  
        <v-spacer></v-spacer>
  
        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
  
        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </v-toolbar>
  </v-card>
    <v-container>
      <v-tabs fixed-tabs>
          <v-tab href="#data-form">
            DATA FORM
          </v-tab>
          <v-tab-item value="data-form">
            <v-card class="pa-3" outlined>
              <data-form/>
            </v-card>
          </v-tab-item>

          <v-tab href="#data-table">
            DATA-TABLE
          </v-tab>
          <v-tab-item value="data-table">
            <v-card>
              <data-table/>
            </v-card>
          </v-tab-item>

        </v-tabs>
    </v-container>
    
  </v-app>
</template>

<script>
import DataForm from './components/DataForm.vue'
import DataTable from './components/DataTable.vue'
export default {
  name: 'App',
  components: {
    DataForm, DataTable
  },
};
</script>
