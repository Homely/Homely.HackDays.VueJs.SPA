<template>
  <div id="app">
    <header>
      <div class="main">
        <img src="https://homely-web.appiancdn.com/assets/images/homely-logo.svg?2844.0.0.0" alt="Homely logo">
      </div>
      <div class="search">
        <input placeholder="search locations" />
        <button v-on:click="getListings">Search</button>
      </div>
    </header>
    <main>
      <aside class="sidebar">
        <h1>Listings</h1>
        <router-link
          v-for="listing in listings"
          active-class="is-active"
          class="link"
          :to="{ name: 'listing', params: { id: listing.id } }">
          {{listing.location.address}}, {{listing.location.suburb}}, {{listing.location.stateCode}}
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
        listings: [],
        endpoint: 'https://api.homely.com.au/listings/location/list?json=',
      }
    },
    methods: {
      getListings(e) {
        e.preventDefault();
        const json = {
          "sort":5,
          "filter": {
            "mode":1,
            "locationId":5714663,
            "wellKnownText":"POLYGON((145.81387037252557 -38.05629730022885, 145.81387037252557 -38.28221702802891, 146.0511063954748 -38.28221702802891, 146.0511063954748 -38.05629730022885, 145.81387037252557 -38.05629730022885))",
          },
          "paging":{"skip":0,"take":24}
        };
        axios.get(this.endpoint + JSON.stringify(json))
          .then(response => {
            this.listings = response.data.items;
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
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'effra', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
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
button {
  border: 0;
  background-color: #fd4974;
  color: #fff;
  font-weight: 700;
  -webkit-appearance: none;
  display: inline-block;
  padding: 0 20px;
  line-height: 40px;
  font-size: 16px;
  white-space: nowrap;
  vertical-align: middle;
  cursor: pointer;
  border-radius: 4px;
  text-decoration: none;
  text-align: center;
}
header {
  position: fixed;
  top: 0;
  width: 100%;
}
.main {
  height: 48px;
  border-bottom: 1px solid #dfe8ee;
  img {
    margin: 8px 16px 0;
  }
}
.search {
    position: relative;
    padding: 0 16px;
    height: 64px;
    white-space: nowrap;
    background-color: #fafafa;
    box-shadow: 1px 1px 4px 0 rgba(0,0,0,.1);
  input {
    display: inline-block;
    margin: 10px 8px 0 0;
    padding: 0 32px 0 12px;
    -webkit-appearance: none;
    min-height: 40px;
    padding: 0 8px;
    border: 1px solid #dfe8ee;
    border-radius: 4px;
    font-size: 16px;
    line-height: 24px;
    text-align: left;
    background: #fff;
    color: #3c515b;
    position: relative;
  }
}
main {
  display: flex;
  height: calc(100vh - 90px);
  width: 100%;
  margin-top: 40px;
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
