<template>
  <div id="app">
    <header>
      <div class="main">
        <img src="https://homely-web.appiancdn.com/assets/images/homely-logo.svg?2844.0.0.0" alt="Homely logo">
      </div>
      <div class="search">
        <input v-on:keyup.enter="getListings" ref="searchBox" placeholder="search locations" />
        <button v-on:click="getListings">Search</button>
      </div>
    </header>
    <main>
      <aside class="sidebar">
        <h1 ref="searchTitle">Search for a location</h1>
        <router-link
          v-for="listing in listings"
          active-class="is-active"
          class="link"
          :to="{ name: 'listing', params: { id: listing.id } }">
          <img :src="'//res-2.cloudinary.com/hd1n2hd4y/image/upload/f_auto,fl_lossy,q_auto,c_fill,w_52,h_52,dpr_1.0/' + listing.images[0].identifier" />
          <b>{{listing.info.price.longDescription}}</b><br/>
          <div>{{listing.location.address}}</div>
          <i>{{listing.mainFeatures.bedrooms}} beds, {{listing.mainFeatures.bathrooms}} baths, {{listing.mainFeatures.carSpaces}} cars</i>
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
        locationEndpoint: 'https://api.homely.com.au/search/locations?q=',
        listingListEndpoint: 'https://api.homely.com.au/listings/location/list?json=',
      }
    },
    methods: {
      getListings(e) {
        e.preventDefault();
        const searchTerm = this.$refs.searchBox.value.toLowerCase();
        const searchTitle = this.$refs.searchTitle;
        searchTitle.innerText = 'Searching...';
        axios.get(this.locationEndpoint + searchTerm).then(response => {
          if (response.data.locations) {
            const json = {
              "sort":5,
              "filter": {
                "mode":1,
                "locationId": response.data.locations[0].id,
              },
              "paging": {
                "skip":0,
                "take":24,
              }
            };
            axios.get(this.listingListEndpoint + JSON.stringify(json))
              .then(response => {
                this.listings = response.data.items;
                const location = this.listings.length ? this.listings[0].location : null;
                if (location) {
                  searchTitle.innerText = `Homes for sale in ${location.suburb}, ${location.stateCode}`;
                } else {
                  searchTitle.innerText = 'Search for a location';
                }
              })
              .catch(error => {
                console.log('-----error-------');
                console.log(error);
              });
          } else {
            this.listings = [];
          }
        });
       
      }
    }
  }
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  line-height: auto;
  overflow: hidden;
}
#app {
  font-family: 'effra', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #3c515b;
  overflow: hidden;
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
  margin-top: 113px;
  margin-left: auto;
  margin-right: auto;
  overflow: hidden;
}
aside {
  flex: 1 0 20%;
  height: 100%;
  overflow-y: auto;
  width: auto;
  padding: 10px;
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
  padding: 8px;
  margin: 0;
  border-radius: 3px;
  position: relative;
  img {
    float: left;
    margin-right: 8px;
  }
  div {
    position: relative;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  i {
    font-size: 12px;
    font-style: inherit;
    color: #666;
  }
  &--home {
    text-transform: uppercase;
    margin-bottom: 30px;
  }
  &.is-active, &:hover {
    background: #fd4974;
    color: #fff;
    i {
      color: #eee;
    }
  }
}
</style>
