<!-- eslint-disable max-len -->
<template>
  <div>
    <div class="q-pa-md gt-sm">
      <div class="q-pa-md q-gutter-sm row justify-center">
        <!-- <q-btn align="center" class="btn-fixed-width" color="cyan-8" label="Explore" icon="travel_explore" /> -->
        <q-btn to="/Restaurants" align="right" class="btn-fixed-width" color="cyan-8" label="Restaurants"
          icon="restaurant" />
        <q-btn to="/Stores" align="between" class="btn-fixed-width" color="cyan-8" label="Stores" icon="store" />
        <q-btn to="/DealBreaker" align="between" class="btn-fixed-width" color="red-6" label="Deal Breaker"
          icon="local_fire_department" />
      </div>
    </div>
    <div class="q-gutter-md row">
      <h4>
        <b>
          &#128293;SUPER HOT DEALS&#128293;
        </b>
      </h4>
      <q-space />
      <div class="q-pa-md q-gutter-sm">
        <q-btn class="text-red" label="Filter" color="blue-grey-9" icon="filter_alt" @click="filterdialog = true" />
        <q-dialog v-model="filterdialog">
          <dealBreakerFilter></dealBreakerFilter>
        </q-dialog>
      </div>
    </div>
    <div class="q-pa-md">
      <div class="row q-col-gutter-sm">
        <div class="col-xs-12 col-sm-6 col-md-4" v-for="deal in deals" :key="deal.store">
          <cardDeal :store="deal.storename" :image="deal.image" :description="deal.store_desc" :message="deal.message"></cardDeal>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from 'vue';
import axios from 'axios';
import cardDeal from 'components/cardDeal.vue';
import dealBreakerFilter from 'src/components/DealBreakerFilter.vue';

export default {
  name: 'StoresPage',
  components: {
    cardDeal,
    dealBreakerFilter,
  },
  data: () => ({
    deals: [],
  }),
  methods: {
    goRestaurantsPage() {
      this.$router.push('/Restaurants');
    },
  },
  mounted() {
    axios.get('http://localhost:3000/store/getDealBrekerStore').then((response) => {
      this.deals = response.data;
    });
    // console.log(this.deals);
  },
  setup() {
    return {
      filterdialog: ref(false),
    };
  },
};

</script>

<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Carter+One&display=swap')

div
  color: $red-6
  background-color: $grey-10
  font-family: 'Carter One', cursive
</style>
