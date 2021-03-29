<template>
  <div class="restaurant-list">
      
        <!-- <ul v-for="(item,index) in dataList" :key="index">
          <li>{{ item.name}},{{item.formatted_address}}</li>
        </ul> -->
        
  </div>
</template>

<script>
import RestaurantCard from '../components/RestaurantCard'
export default {
  components: {
    RestaurantCard
  },
  props: ['dataList']
}
</script>
<style lanh="scss">
.restaurant-list {
  padding: 20px 0;
}
</style>