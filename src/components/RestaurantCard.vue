<template>
  <div class="Restaurant-card">
    <b-card
    :title="restaurantList.name">
        <b-card-text>
            <b>Address:</b> {{ item.formatted_address }}<br>
            <b>Phone Number:</b> {{ item.formatted_phone_number }}
        </b-card-text>
        <b-card-text >
          <b> Rating: {{ item.rating }} from {{ item.user_ratings_total }} </b>
        </b-card-text>
        <b-card-text >
            <a class="link-google" target="_blank" :href="item.url">View on Google Maps</a>
        </b-card-text>
    </b-card>
  </div>
</template>

<script>
export default {
    data() {
        return {
           restaurantList: this.item,
        }
    },
    props: [
        'item'
    ]
}
</script>
<style lang="scss">
.Restaurant-card {
    .card {
        background-color: #fcf1da;
        margin: 15px 0;
        border: 1px solid #e38c4c;
        border-radius: 10px;
        color: #977b60;
        .card-title {
            font-size: 1.3rem;
        }
        a {
            // color: #977b60;
        }
    }
}
</style>