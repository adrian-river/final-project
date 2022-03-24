<template>
  <div>
    <div v-for="cine in cines" :key="cine.id">
      <p>
        {{ cine.attributes.cine_name }}
      </p>
      <ul>
        <li v-for="movie in cine.attributes.movies.data" :key="movie.id">
          {{ movie.attributes.movie_name }}
        </li>
        <li v-for="movie in cine.attributes.movies.data" :key="movie.id">
          {{ movie.attributes.sypnosis }}
        </li>
        <li v-for="movie in cine.attributes.movies.data" :key="movie.id">
          {{ movie.attributes.image }}  
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      cines: [],
      error: null,
      headers: {'Content-Type': 'application/json'}
    }
  },
  methods: {
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    }
  },
  async mounted () {
    try {
      const response = await fetch("https://e795-38-25-16-118.ngrok.io/api/cines?populate=* ", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
        .then(this.parseJSON);
        this.cines = response.data
        console.log(this.cines.data)
    } catch (error) {
      this.error = error
    }
  }
}
</script>
