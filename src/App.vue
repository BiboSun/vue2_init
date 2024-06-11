<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <button @click="fetchData">点击获取数据</button>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
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