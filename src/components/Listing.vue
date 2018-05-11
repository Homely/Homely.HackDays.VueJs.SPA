<template lang="html">
  <div class="listing" v-if="listing">
    <h1 class="listing__title">{{listing.location.address}}, {{listing.location.suburb}}, {{listing.location.stateCode}}</h1>
    <p class="listing__body">{{ listing.description }}</p>
    <p class="listing__id">{{ listing.id }}</p>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    props: ['id'],
    metaInfo() {
      return {
        title: this.listing && this.listing.title,
      };
    },
    data() {
      return {
        listing: null,
        endpoint: 'https://api.homely.com.au/listing/',
      }
    },
    methods: {
      getListing(id) {
        axios(this.endpoint + id)
          .then(response => {
            this.listing = response.data.listing
          })
          .catch( error => {
            console.log('-----error-------');
            console.log(error)
          })
      }
    },
    
    created() {
      this.getListing(this.id);
    },
    watch: {
      '$route'() {
        this.getListing(this.id);
      }
    }
  }
</script>

<style lang="scss" scoped>
  .listing {
    position: relative;
    max-width: 500px;
    margin: 0 auto;
    padding: 50px 20px 70px;
    &__title {
      position: relative;
      text-transform: uppercase;
      z-index: 1;
    }
    &__body {
      position: relative;
      z-index: 1;
    }
    &__id {
      position: absolute;
      font-size: 280px;
      bottom: -50px;
      margin: 0;
      color: #eeeeee;
      right: -20px;
      line-height: 1;
      font-weight: 900;
      z-index: 0;
    }
  }
</style>