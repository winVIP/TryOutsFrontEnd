<template>
  <div>
    <h1>Movie Search</h1>
    <b-form id="MovieSearchForm" @submit="onSubmit">
      <b-form-group
        id="input-group-1"
        label="Movie title:"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          v-model="form.title"
          placeholder="Enter movie title"
          required
        ></b-form-input>
      </b-form-group>
      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
    <br>
    <div id="MovieSearchTable" v-if="showSearchTable">
        <b-table bordered hover :items="items" :fields="fields" @row-clicked="onRowClick"></b-table>    
    </div>
    <b-alert v-model="showErrorMessage" variant="danger" dismissible>
        Couldn't find any movies
    </b-alert>
    <b-modal hide-footer id="MovieDetailsModal">
      <template #modal-title>
        Movie Details
      </template>
      <b-table borderless stacked :items="movieDetails" :fields="movieDetailFields"></b-table>
      <b-button class="mt-3" block @click="$bvModal.hide('MovieDetailsModal')">Close</b-button>
    </b-modal>
    <b-modal hide-footer id="MovieDetailsModalError">
      <template #modal-title>
        Movie Details
      </template>
      Didn't find any details about the movie
      <b-button class="mt-3" block @click="$bvModal.hide('MovieDetailsModalError')">Close</b-button>
    </b-modal>
  </div>
</template>

<script>
  import axios from "axios"
  export default {
    data() {
      return {
        form: {
          title: '',
        },
        items: [],
        fields: [
            { key: "title", label: "Title"},
            { key: "year", label: "Year"},
        ],
        showSearchTable: false,
        showErrorMessage: false,
        movieDetails: [],
        movieDetailFields: [
          { key: "title", label: "Title"},
          { key: "year", label: "Year"},
          { key: "imdbRating", label: "IMDB rating"},
          { key: "genre", label: "Genre"},
          { key: "plot", label: "Plot"},
          { key: "actors", label: "Main actors"}
        ]
      }
    },
    methods: {
      async onSubmit(event) {
        event.preventDefault()

        try{
          const response = await axios.get("https://localhost:5001/api/Movie/Search/" + encodeURIComponent(this.form.title))

          this.items = response.data.map(x => {
            return {
                title: x.title,
                year: x.year,
                imdbID: x.imdbID
            }
          })
          this.showSearchTable = true
          this.showErrorMessage = false
        }
        catch{
          this.showSearchTable = false
          this.showErrorMessage = true
        }
      },
      async onRowClick(item){
        try{
          const response = await axios.get("https://localhost:5001/api/Movie/" + encodeURIComponent(item.imdbID))

          this.$bvModal.show("MovieDetailsModal")
          this.movieDetails = [response.data]
        }
        catch{
          this.$bvModal.show("MovieDetailsModalError")
        }
      }
    },
    
  }
</script>

<style>
    #MovieSearchForm {
        margin-left: 30%;
        margin-right: 30%;
    }
    #MovieSearchTable {
        margin-left: 10%;
        margin-right: 10%;
    }
</style>