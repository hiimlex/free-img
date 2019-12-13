<template>
  <v-container class="mx-auto" v-if="photos">
    <v-row>
      <v-col cols="12">
        <v-row justify="center">
          <v-btn @click="random">Random</v-btn>
        </v-row>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="12" md="6" lg="4" xl="3" v-for="photo in photos" :key="photo.id">
        <v-card class="mx-auto" max-width="400">
          <v-img height="300" width="400" :src="photo.download_url"></v-img>
          <v-card-actions>
            <v-card-title>By {{photo.author}}</v-card-title>
            <v-spacer></v-spacer>
            <a :href="photo.download_url" class="text-decoration">
              <v-btn icon>
                <v-icon>mdi-download</v-icon>
              </v-btn>
            </a>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      page: 1,
      photos: false,
      picsum: ""
    };
  },
  created() {
    axios
      .get("https://picsum.photos/v2/list")
      .then(response => {
        this.photos = response.data;
      })
      .catch(() => {});
  },
  methods: {
    random() {
      this.page = Math.floor(Math.random() * 11);
      this.getPics();
    },
    getPics() {
      axios
        .get("https://picsum.photos/v2/list?page=" + this.page + "&limit=30")
        .then(response => {
          this.photos = response.data;
        })
        .catch(() => {});
    }
  }
};
</script>

<style>
.text-decoration {
  text-decoration: none;
}
</style>
