<template>
  
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      cine: [],
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
        this.cine = response
        console.log(this.cine)
    } catch (error) {
      this.error = error
    }
  }
}
</script>
