<template>
  <v-container>
    <v-app-bar app color="#03081a" dark>
      <v-row>
        <v-col cols="3" class="d-flex">
          <v-text-field v-model="name" label="Enter Name" ></v-text-field>
          <v-btn @click="searchName">Search</v-btn>
        </v-col>
      </v-row>
    </v-app-bar>

    <v-row class="" justify="center">
      <v-col cols="11"> </v-col>

      <v-col cols="11" v-for="uni in unis" :key="uni.name">
        <v-card id="card" class="mx-auto" max-width="800" outlined>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1 text-white">
                {{ uni.name }} ({{ uni.acronym }})</v-list-item-title
              >
              <v-list-item-subtitle>{{ uni.motto }}</v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-avatar tile width="80" class="mt-9">
              <img :src="uni.logo" alt="" />
            </v-list-item-avatar>
          </v-list-item>

          <v-card-actions>
            <a :href="uni.web">
              <v-btn>Visit</v-btn>
            </a>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <v-col col="12">
      <div class="text-center">
        <v-btn @click="next"><v-icon>mdi-chevron-left</v-icon></v-btn>
        <v-btn @click="prev"><v-icon>mdi-chevron-right</v-icon></v-btn>
      </div>
    </v-col>

    <div class="text-center ma-2">
      <v-snackbar v-model="snackbar">
        University not found
        <template v-slot:action="{ attrs }">
          <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    unis: null,
    name: null,
    snackbar: false,
    index: 0,
    allData: []
  }),

  mounted() {
    fetch("https://nigerian-universities.herokuapp.com/")
      .then((response) => response.json())
      .then((data) => {
        this.allData = data;
        this.unis = data;
        const n = 26;
        const result = new Array(Math.ceil(this.unis.length / n))
          .fill()
          .map(() => this.unis.splice(0, n));
        this.unis = result[this.index];
      });
  },
  methods: {
    searchName() {
      console.log(this.allData)
      var outputs = this.allData;
      var final = [];
      outputs.forEach((number) => {
        if (number.name.includes(this.name)) {
          final.push(number);
          this.unis = final;
        } else {
          // this.snackbar = true;
        }
      });
    },
    next (){
      this.index = this.index +1;
      fetch("https://nigerian-universities.herokuapp.com/")
      .then((response) => response.json())
      .then((data) => {
        this.unis = data;
        const n = 26;
        const result = new Array(Math.ceil(this.unis.length / n))
          .fill()
          .map(() => this.unis.splice(0, n));
        this.unis = result[this.index];
      });
    },
    prev(){
      if(this.index > 0){
        this.index = this.index-1;
        fetch("https://nigerian-universities.herokuapp.com/")
      .then((response) => response.json())
      .then((data) => {
        this.unis = data;
        const n = 26;
        const result = new Array(Math.ceil(this.unis.length / n))
          .fill()
          .map(() => this.unis.splice(0, n));
        this.unis = result[this.index];
      });
      }else{
        console.log("bad")
      }
    },
  },
    computed:{
    
    }
};
</script>
<style>
a {
  text-decoration: none !important;
}

/* 
  #app{
    background-color: #03081a!important;
  }
  #card{
    background-color: #03081a!important;
  } */
</style>
