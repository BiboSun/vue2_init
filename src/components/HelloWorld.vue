<template>
  <div id="app1">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <button @click="fetchData">点击获取数据</button>
    </nav>
    <router-view/>
  </div>
</template>

<script>

export default {
  data () {
    return {
      message: ''
    }
  },
  methods: {
    async fetchData () {
      try {
        const response = await this.$axios.get('/api/testJob')
        this.message = response.data.message
      } catch (error) {
        console.error('Error fetching data:', error)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
