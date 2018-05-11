<template lang="html">
  <div ref="listingContainer" class="listing" v-if="listing">
    <h1 class="listing__title">
      <b>{{listing.info.price.longDescription}}</b>
      {{listing.location.address}}, {{listing.location.suburb}}, {{listing.location.stateCode}}
      <i>{{listing.mainFeatures.bedrooms}} beds, {{listing.mainFeatures.bathrooms}} baths, {{listing.mainFeatures.carSpaces}} cars</i>
    </h1>
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
            const panelWidth = this.$refs.listingContainer.scrollWidth;
            this.$refs.listingContainer.style.backgroundImage = `url(//res-2.cloudinary.com/hd1n2hd4y/image/upload/f_auto,fl_lossy,q_auto,c_fill,w_${panelWidth},dpr_1.0/${this.listing.images[0].identifier})`;
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
    width: 100%;
    height: 100%;
    background-color: #3c515b;
    background-repeat: no-repeat;
    background-size: 100%;
    margin: 0 auto;
    padding: 0;
    &__title {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      color: #fff;
      z-index: 1;
      padding: 80px 40px 40px;
      background: linear-gradient(180deg,transparent,#000);
      font-weight: 500;
      b {
        font-weight: 600;
        display: block;
        font-style: inherit;
        margin: 0 0 10px;
      }
      i {
        margin: 10px 0 0;
        display: block;
        font-size: 20px;
        font-style: inherit;
        font-weight: 400;
      }
    }
  }
</style>