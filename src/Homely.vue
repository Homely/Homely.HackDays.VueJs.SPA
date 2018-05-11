<template>
  <div id="app">
    <header>
      <h1>Homely Vue.js</h1>
        <input placeholder="search locations" />
        <button onClick={search()}>Search</button>
    </header>
    <main>
      <aside class="sidebar">
        <router-link
            v-for="post in posts"
            active-class="is-active"
            class="link"
            :to="{ name: 'post', params: { id: post.id } }">
          {{post.id}}. {{post.title}}
        </router-link>
      </aside>
      <div class="content">
        <router-view></router-view>
      </div>
    </main>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data () {
      return {
        posts: [],
        endpoint: 'https://jsonplaceholder.typicode.com/posts/',
      }
    },
    created() {
      this.getAllPosts();
    },
    search(e) {
      e.preventDefault();
      
    },
    methods: {
      getAllPosts() {
        axios.get(this.endpoint)
          .then(response => {
            this.posts = response.data;
          })
          .catch(error => {
            console.log('-----error-------');
            console.log(error);
          })
      }
    }
  }
</script>

<style lang="scss">
#app {
  font-family: 'effra', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #3c515b;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
  color: #fd4974;
}

  header {
    position: fixed;
    top: 0;
    width: 100%;
    min-height: 90px;
    text-align: center;
  }
  main {
    display: flex;
    height: calc(100vh - 90px);
    max-width: 1200px;
    margin-top: 90px;
    margin-left: auto;
    margin-right: auto;
    overflow: hidden;
  }
  aside {
    flex: 1 0 30%;
    height: 100%;
    overflow-y: auto;
    width: 30%;
    padding: 50px 30px;
    box-sizing: border-box;
    text-align: left;
  }
    .content {
    flex: 1 1 70%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .link {
    display: block;
    text-decoration: none;
    margin-bottom: 10px;
    color: #2c3e50;
    &--home {
      text-transform: uppercase;
      margin-bottom: 30px;
    }
    &.is-active {
      color: #fd4974;
    }
  }
</style>
