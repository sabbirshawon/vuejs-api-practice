<template>
  <v-container>
    <v-row>
      <v-col>
        <div class="d-flex">
          <v-text-field
            label="Search"
            append-icon="mdi-magnify"
            solo
            v-model="searchString"
          ></v-text-field>
          <v-btn
            class="ml-4"
            color="primary"
            elevation="2"
            large
            @click="SearchArtist"
          >
            Search
          </v-btn>
        </div>
      </v-col>
    </v-row>
    <v-row v-if="artists.length">
      <v-col>
        <v-card class="mx-auto" tile>
          <v-list dense>
            <v-subheader
              >Artist for Search Result {{ searchString }}</v-subheader
            >
            <v-list-item-group color="primary">
              <v-list-item
                v-for="(item, i) in artists"
                :key="i"
                :to="{ name: 'About', params: { id: item.name } }"
              >
                <v-list-item-content>
                  <v-list-item-title v-text="item.name"></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
// import HelloWorld from '../components/HelloWorld'
import config from './config'
const apiKey = config.apiKey
import axios from 'axios'

export default {
  name: 'Home',

  components: {},
  data: function() {
    return {
      searchString: '',
      artists: [],
    }
  },
  methods: {
    SearchArtist() {
      if (this.searchString) {
        axios
          .get(
            `https://ws.audioscrobbler.com/2.0/?method=artist.search&api_key=${apiKey}&format=json`,
            {
              params: {
                artist: this.searchString,
              },
            }
          )
          .then((response) => {
            this.artists = response.data.results.artistmatches.artist
            console.log(response)
          })
          .catch((error) => {
            console.log(error)
          })
      }
    },
  },
}
</script>
