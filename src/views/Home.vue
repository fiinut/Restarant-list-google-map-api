<template>
  <div class="home container">
    <div class="header-bar flex-container">
      <div class="title">
        <span> Restaurants </span>
      </div>
      <div class="search-menu">
          <b-form-input class="input-location" v-model="searchInput" id="search-input" @focus="searchInput = ''"></b-form-input>
      </div>
    </div>
    <RestaurantCard v-for="(item,index) in this.dataList" :key="index" :item="item"></RestaurantCard>
    <div id="map"></div>
  </div>
</template>

<script>
import GoogleMapsLoader from 'google-maps'
import RestaurantCard from '../components/RestaurantCard'
export default {
  name: 'Home',
  data(){
    return {
      map: null,
      dataList: [],
      searchInput: 'Bange Sue',
      locationSearch: { lat: 13.8235283, lng: 100.5081204 }, //set default lat,long value of 'bang sue'
      typeSearch: "restaurant",
      inputLocation: ''
    }
  },
  components: {
    RestaurantCard
  },
  watch: {
    searchInput(value){
      if(value){
        this.placeSearchAutoComplete()
      }
    }
  },
  mounted(){
    this.getRestaurantList() //Call func for restaurant list of default value 'Bang sue'
  },
  methods: { // Function for get autocomplete of address for google map api
    placeSearchAutoComplete() {
      const map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: -33.8688, lng: 151.2195 },
        zoom: 13,
        mapTypeId: "roadmap",
      });
      const input = document.getElementById("search-input");
      const autocomplete = new google.maps.places.Autocomplete(input);
      autocomplete.bindTo("bounds", map);
      autocomplete.setFields(["place_id", "geometry", "name"]);
      autocomplete.addListener("place_changed", () => {
        const place = autocomplete.getPlace();
        if (place.geometry.location) {
          this.locationSearch = place.geometry.location //Defind lat,long value to 'locationSearch' variable
          this.getRestaurantList() // Call this function for get detail Restaurant List
        }
      })
    },
    getRestaurantList(){ // Function for get list restaurant from google map api.      
      this.dataList = []
      let map
      let service

      map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: 13.8108407, lng: 100.5544372 },
        zoom: 14,
      });
      // Create variable 'request' for set value to api
      const request = {
        location: this.locationSearch,
        radius: 1000,
        type: this.typeSearch
      };
      service = new google.maps.places.PlacesService(map);
      service.textSearch(request, (results, status) => {
        if (status === google.maps.places.PlacesServiceStatus.OK && results) {
          results.forEach((e =>{
            this.placeDetail(e.place_id) //Call function 'placeDetail' by 'place_id'
          }))
        }
      });
    },
    placeDetail(placeId) { // Function for Get more detail of restaurant.
      const map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: -33.866, lng: 151.196 },
        zoom: 15,
      });
      const request = {
        placeId: placeId,
        fields: ["ALL"] //Defind target fields you want.
      };
      const service = new google.maps.places.PlacesService(map);
      service.getDetails(request, (place, status) => {
        if (status === google.maps.places.PlacesServiceStatus.OK && place) {
          this.dataList.push(place)
        }
      })
    }
  }
}
</script>
<style lang="scss">
#map {
  height: 600px;
  background-color: gray;
  display: none;
}
.header-bar {
  color: #977b60;
  display: flex;
  justify-content: space-between;
  height: auto;
  align-items: center;
  font-weight: 500;
  font-size: 2rem;

  @media only screen and (max-device-width: 480px) {
    display: block;
    text-align: center;
  }
}
.search-menu {
  display: flex;
  align-items: center;
  .input-location {
    border: 1px solid #e38c4c;
  }
  .form-control {
    color: #977b60;
    font-weight: 500;
    background-color: #fcf1da;
  }
  .button-search {
    background-color: #f6c561;
    border: none;
    border-radius: 10px;
  }
}
</style>
