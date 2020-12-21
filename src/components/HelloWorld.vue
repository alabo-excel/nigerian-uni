<template>
  <v-container>
    <v-app-bar app color="#03081a" dark>
      <v-row class="mt-6">
        <v-col cols="5" class="">
          <h2>9jaUni</h2>
        </v-col>
        <v-col cols="7" class="d-flex">
          <v-text-field
            @blur="searchName"
            v-model="name"
            label="Enter Name"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-app-bar>
    <p id="nav"></p>
    <v-row class="" justify="center">
      <v-col cols="12" justify="center" align="center">
        <v-progress-circular
          :size="70"
          :width="7"
          color="#03081a"
          indeterminate
          id="spinner"
        ></v-progress-circular>
      </v-col>

      <v-col lg="6" sm="11" v-for="uni in unis" :key="uni.name">
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
              <v-btn id="btn" color="#03081a" class="text--primary">Visit</v-btn>
            </a>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <v-col col="12">
      <div id="pagination" class="text-center">
        <a href="#nav">
          <v-btn class="mr-5" color="#03081a" id="btn" @click="next"
            ><v-icon>mdi-chevron-left</v-icon></v-btn
          >
        </a>
        <a href="#nav">
          <v-btn class="ml-5" color="#03081a" id="btn" @click="prev"
            ><v-icon>mdi-chevron-right</v-icon></v-btn
          >
        </a>
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
    allData: [],
  }),

  mounted() {
    document.getElementById("pagination").style.display = "none";
    document.getElementById("spinner").style.display = "block";
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
        document.getElementById("spinner").style.display = "none";
        document.getElementById("pagination").style.display = "block";
      });
  },
  methods: {
    searchName() {
      document.getElementById("spinner").style.display = "block";
      fetch("https://nigerian-universities.herokuapp.com/")
        .then((response) => response.json())
        .then((data) => {
          this.allData = data;
          var outputs = this.allData;
          var final = [];
          outputs.forEach((number) => {
            if (number.name.includes(this.name)) {
              final.push(number);
              this.unis = final;
              // this.snackbar = false;
            } else {
              // this.snackbar = true;
            }
          });
          document.getElementById("spinner").style.display = "none";
        });
    },
    next() {
      document.getElementById("spinner").style.display = "block";
      if (this.index < 5) {
        this.index = this.index + 1;
        fetch("https://nigerian-universities.herokuapp.com/")
          .then((response) => response.json())
          .then((data) => {
            this.unis = data;
            const n = 26;
            const result = new Array(Math.ceil(this.unis.length / n))
              .fill()
              .map(() => this.unis.splice(0, n));
            this.unis = result[this.index];
            document.getElementById("spinner").style.display = "none";
          });
      } else {
        console.log("bad");
      }
    },
    prev() {
      document.getElementById("spinner").style.display = "block";
      if (this.index > 0) {
        this.index = this.index - 1;
        fetch("https://nigerian-universities.herokuapp.com/")
          .then((response) => response.json())
          .then((data) => {
            this.unis = data;
            const n = 26;
            const result = new Array(Math.ceil(this.unis.length / n))
              .fill()
              .map(() => this.unis.splice(0, n));
            this.unis = result[this.index];
            document.getElementById("spinner").style.display = "none";
          });
      } else {
        console.log("bad");
      }
    },
  },
  computed: {},
};
</script>
<style>
a {
  text-decoration: none !important;
}
#btn {
  color: white !important;
}

/* 
  #app{
    background-color: #03081a!important;
  }
  #card{
    background-color: #03081a!important;
  } */
</style>
