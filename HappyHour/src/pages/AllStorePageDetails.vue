<!-- eslint-disable max-len -->
<template>
  <div>
    <div class="image-container" style="height:250px;">
      <q-img class="image" :src=store_image no-native-menu>
        <div class="absolute-bottom text-subtitle1 text-center">
          {{ storeName }}
        </div>
      </q-img>

    </div>
    <div class="q-pa-md">
      <q-layout view="lHh lpr lFf" container style="height: 800px" class="shadow-2 rounded-borders">
        <q-header elevated>
          <q-bar>
            <q-icon name="schedule" color="blue-grey-8" />
            <div style="color: #36486b;">Open until 22:00</div>
            <div class="q-pa-md q-gutter-sm">
              <q-btn label="See more information" color="blue-grey-8" icon="info" @click="moreinfodialog = true" />
              <q-dialog v-model="moreinfodialog">
                <seeMoreInfo :store=storeName :description=store_desc :store_phone=store_phone
                  :street_address=street_address :post_code=post_code>

                </seeMoreInfo>
              </q-dialog>
            </div>

            <q-space />
            <div class="q-pa-md gt-xs" color="blue-grey-8" style="max-width: 300px;max-height: 120px;">
              <div class="q-gutter-md">
                <q-input color="blue-grey-8" v-model="search" debounce="1000" borderless placeholder="Search">
                  <template v-slot:append>
                    <q-icon color="blue-grey-8" name="search" />
                  </template>
                </q-input>
              </div>
            </div>

          </q-bar>
          <div class="q-pa-sm q-pl-md row items-center lt-sm">
            <div class="q-pa-md" style="max-width: 300px;max-height: 50px;">
              <div class="q-gutter-md">
                <q-input v-model="search" debounce="1000" borderless placeholder="Search in {{ storeName }}">
                  <template v-slot:append>
                    <q-icon name="search" />
                  </template>
                </q-input>
              </div>
            </div>
          </div>
        </q-header>
        <h2>Store Deals</h2>
        <div class="q-pa-md">

          <div class="row q-col-gutter-sm">
            <q-card class="my-card col-xs-12 col-sm-6 col-md-4" v-for="deal in deals" :key="deal._id">
              <q-img :src=deal.image style="height: 250px; max-width: 625px" />
              <q-card-section>
                <q-chip dense color="red" text-color="white" :label="`${deal.deal}%`" />
                <div class="text-h6">{{ deal.dealfor[0] }}</div>
                <div class="float-right">
                  <div v-if="getIcon(deal) === 'thumb_up'">
                    <span>{{ deal.like_counter }}/{{ deal.like }}</span> <br>
                    <q-btn color="cyan-8" :icon=getIcon(deal) @click="addlike(deal)"></q-btn>
                  </div>
                  <div v-else>
                    <q-btn color="cyan-8" :icon=getIcon(deal) @click="addToBasket(deal.dealfor[0])"></q-btn>
                  </div>
                </div>
                <div class="text-grey-8 text-subtitle2">{{ deal.days[0].from }}-{{ deal.days[0].to }}</div>
                <div class="text-blue-grey-8 text-subtitle2">Price: {{ deal.price }}$</div>
              </q-card-section>
            </q-card>
            <q-space />
          </div>

        </div>
        <h2>Store Menu</h2>
        <div class="row q-col-gutter-sm">
          <q-card class="my-card col-xs-12 col-sm-6 col-md-4" v-for="item in menu" :key="item.name">
            <q-img :src=item.image style="height: 250px; max-width: 625px" />
            <q-card-section>
              <div class="text-h6">{{ item.name }}</div>
              <div class="float-right"><q-btn color="cyan-8" icon="shopping_cart" @click="addToBasket(item.name)"></q-btn>
              </div>
              <div class="text-grey-8 text-subtitle2">{{ item.des }}</div>
              <div class="text-blue-grey-8 text-subtitle2">{{ item.price }}$</div>
            </q-card-section>
          </q-card>
        </div>
        <h2>Customer Review</h2>
        <h4>Picture Your Experience</h4>
        <div class="q-pa-md">
          <h5>Your Name</h5>
          <q-input rounded outlined v-model="avatarname" style="max-width: 300px;" />
        </div>
        <div class="q-pa-md">
          <h5>Your Photo</h5>
          <q-file v-model="avatarImage" label="Upload Photo" filled style="max-width: 300px"
            @change="updateAvatarFile()" />
          <q-img :src="fileAvatarUrl" spinner-color="white" style="height: 140px; max-width: 150px" />
        </div>
        <div class="q-pa-md">
          <h5>Your Review Photo</h5>
          <q-file v-model="image" label="Upload Photo" filled style="max-width: 300px" @change="updateFile()" />
          <q-img :src="fileUrl" spinner-color="white" style="height: 140px; max-width: 150px" />
        </div>
        <div class="q-pa-md row justify-center bg-grey-3 shadow">
          <div style="width: 100%; max-width: 400px">
            <q-chat-message name="Ana" avatar="https://cdn.quasar.dev/img/avatar3.jpg" text-color="white"
              bg-color="blue-grey-8" label="8th June 2023" size="1000">

              <div>
                <img style="width: 100%;max-with:100px;"
                  src="https://images.squarespace-cdn.com/content/v1/5f9decf1e529e27a4705d448/1681676661420-MZGRIQXRH7H87EH70AB9/MacBook+Pro+-+22banner-17.png?format=2500w"
                  class="my-emoticon" />
              </div>
            </q-chat-message>

            <q-chat-message :name="avatarname" :avatar="fileAvatarUrl" label="18th June 2023" bg-color="cyan-8">
              <div>
                <img style="width: 100%;max-with:100px;" :src="fileUrl" class="my-emoticon" />
              </div>
            </q-chat-message>
          </div>
        </div>
        <br>
        <br>
        <br>
        <br>
      </q-layout>
    </div>
  </div>
</template>

<script>
import seeMoreInfo from 'src/components/SeeMoreInfo.vue';
import axios from 'axios';
import { ref } from 'vue';

export default {
  name: 'cardDeal',
  components: {
    seeMoreInfo,
  },
  props: ['storeName'],
  data: () => ({
    city_address: '',
    store_desc: '',
    store_phone: '',
    store_image: '',
    post_code: '',
    street_address: '',
    menu: [],
    storeModel: {},
    deals: {},
    image: ref(null),
    image1Url: ref(''),
    avatarImage: ref(null),
    image2Url: ref(''),
    moreinfodialog: ref(false),
  }),
  methods: {
    getIcon(deal) {
      let icon = 'shopping_cart';
      if (deal.del_type === 'dealBreaker') {
        if (deal.like_counter < deal.like) {
          icon = 'thumb_up';
        }
      }
      return icon;
    },
    changeColor() {
      this.$refs.favoriteButton.color = 'red';
    },
    blobToBase64(blob) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(blob);
      });
    },
    updateFile() {
      console.log('image gggggggggggggggggggggggg');
    },
    updateAvatarFile() {
      console.log('image gggggggggggggggggggggggg');
    },
    getStoreDetails() {
      const body = {};
      body.storename = this.storeName;
      axios.post('http://localhost:3000/store/getStoreData', body).then((response) => {
        console.log('data', response.data);
        this.city_address = response.data[0].city_address;
        this.store_phone = response.data[0].store_phone;
        this.post_code = response.data[0].post_code;
        this.street_address = response.data[0].street_address;
        this.store_image = response.data[0].image;
        this.store_desc = response.data[0].store_desc;
        if (response.data[0].menu === undefined) {
          this.menu = [];
        } else {
          this.menu = response.data[0].menu;
          this.getdeals(response.data[0].menu);
        }
      });
    },
    getdeals(menu) {
      const store = {};
      store.storename = this.storeName;
      axios.get('http://localhost:3000/deals/storeDeals', { params: { store } }).then((response) => {
        this.deals = response.data.valueOf();
        this.deals.forEach((element) => {
          const newobject = element;
          const menuitem = menu.findIndex(({ name }) => name === element.dealfor[0]);
          const itemimage = menu[menuitem].image;
          const { price } = menu[menuitem];
          newobject.image = itemimage;
          newobject.price = (
            parseInt(price, 10) - ((parseInt(price, 10) / 100) * parseInt(newobject.deal, 10)));
          element = newobject;
        });
      });
    },
    addToBasket(menuitem) {
      const body = {};
      body.storename = this.storeName;
      body.menuitem = menuitem;
      axios.post('http://localhost:3000/store/addOrders', body).then((response) => {
        console.log('addToBasket', response.data);
      });
    },
    addlike(deal) {
      const body = {};
      const { _id } = deal;
      console.log('deal', _id);
      body.deal_id = _id;
      axios.post('http://localhost:3000/deals/addlike', body).then((response) => {
        console.log('addlike', response.data);
        this.getStoreDetails();
      });
    },
  },
  mounted() {
    this.getStoreDetails();
  },
  watch: {
    isFavorite(val) {
      this.$refs.favoriteButton.color = val ? 'red' : 'black';
    },
  },
  setup() {
    return {
      // columns,
      // rows,
    };
  },
  computed: {
    fileUrl() {
      let url = '';
      if (this.image !== null) {
        url = URL.createObjectURL(this.image);
        console.log('image gggggggggggggggggggggggg', url);
      }
      return url;
    },
    fileAvatarUrl() {
      let avatarUrl = '';
      if (this.avatarImage !== null) {
        avatarUrl = URL.createObjectURL(this.avatarImage);
        console.log('image gggggggggggggggggggggggg', avatarUrl);
      }
      return avatarUrl;
    },
  },
};
</script>

<style>
.image-container {
  /* height: 50vh; */
  /* hauteur de 25% de l'écran */
  position: relative;
  /* position relative pour contenir l'image */
  text-decoration: none;
  color: #333;
}

.image {
  position: absolute;
  /* position absolue pour couvrir toute la hauteur du conteneur */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  /* pour que l'image s'adapte et ne soit pas déformée */
}
</style>
<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 300px
</style>
