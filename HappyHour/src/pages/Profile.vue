<!-- eslint-disable max-len -->
<template>
  <div>
    <q-page class="q-pa-sm">
      <div class="row q-col-gutter-sm">
        <div class="col-lg-8 col-md-8 col-xs-12 col-sm-12">
          <q-card class="bg-blue-grey-9 card-bg text-white no-shadow" bordered>
            <q-card-section class="text-h6 ">
              <div class="text-h6">Edit Profile</div>
              <div class="text-subtitle2">Complete your profile</div>
            </q-card-section>
            <q-card-section class="q-pa-sm">
              <q-list class="row">
                <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                  <q-item-section side>
                    <q-avatar size="100px">
                      <img src="https://cdn.quasar.dev/img/boy-avatar.png">
                    </q-avatar>
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="user_name" label="User Name" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="email" label="Email Address" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="first_name" label="First Name" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="last_name" label="Last Name" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" autogrow dense v-model="address" label="Address" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="city" label="City" />
                  </q-item-section>
                </q-item>
                <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                  <q-item-section>
                    <q-input dark color="white" dense v-model="post_code" label="Postal Code" />
                  </q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
            <q-card-actions align="right">
              <q-btn class="text-capitalize text-white" rounded color="cyan-8" icon="edit_note" @click=updatePersonalData>
                Update User Info</q-btn>
            </q-card-actions>
          </q-card>
        </div>
        <div class="col-lg-4 col-md-4 col-xs-12 col-sm-12">
          <q-card class="bg-blue-grey-9 card-bg text-white no-shadow" bordered>
            <q-card-section class="text-center bg-transparent">
              <q-avatar size="120px" class="shadow-10">
                <img src="../../public/icons/store.png">
              </q-avatar>
              <div class="text-h6 q-mt-lg">My Stores</div>
            </q-card-section>
            <q-item-section>
              <q-card-actions align="center">
                <q-btn label="New store" class="text-capitalize" rounded color="cyan-8" icon="add"
                  style="max-width: 120px" @click="choosestoredialog = true"></q-btn>
              </q-card-actions>
            </q-item-section>
            <q-card-section>
              <div class="text-body2 text-justify">
                <div class="q-pa-md" style="max-width: 600px">
                  <q-list dense bordered padding class="rounded-borders">
                    <div v-for="store in stores" :key="store.storename">
                      <q-item clickable v-ripple :to="{ path: `/EditStore/${store.storename}` }">
                        <q-item-section>
                          {{ store.storename }}
                        </q-item-section>
                        <q-btn flat rounded icon="edit"></q-btn>
                        <q-btn flat rounded icon="delete"></q-btn>
                      </q-item>
                    </div>
                  </q-list>
                </div>
              </div>
            </q-card-section>
          </q-card>
          <q-dialog v-model="choosestoredialog">
            <ChooseStoreType :refreshStore="refreshStore"></ChooseStoreType>
          </q-dialog>
          <!-- <q-dialog v-model="createstordialog">
            <CreateStore :refreshStore="refreshStore"></CreateStore>
          </q-dialog> -->
        </div>
        <div class="col-lg-8 col-md-8 col-xs-12 col-sm-12">
          <q-card class="bg-blue-grey-9 card-bg text-white no-shadow" bordered>
            <q-card-section class="text-h6 q-pa-sm">
              <div class="text-h6">Change Password</div>
            </q-card-section>
            <q-card-section class="q-pa-sm row">
              <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
                <q-item-section>
                  Current Password
                </q-item-section>
              </q-item>
              <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
                <q-item-section>
                  <q-input type="password" dark dense outlined color="white" round
                    v-model="password_dict.current_password" label="Current Password" />
                </q-item-section>
              </q-item>
              <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
                <q-item-section>
                  New Password
                </q-item-section>
              </q-item>
              <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
                <q-item-section>
                  <q-input type="password" dark dense outlined color="white" round v-model="password_dict.new_password"
                    label="New Password" />
                </q-item-section>
              </q-item>
              <q-item class="col-lg-4 col-md-4 col-sm-12 col-xs-12">
                <q-item-section>
                  Confirm New Password
                </q-item-section>
              </q-item>
              <q-item class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
                <q-item-section>
                  <q-input type="password" dark dense outlined round color="white"
                    v-model="password_dict.confirm_new_password" label="Confirm New Password" />
                </q-item-section>
              </q-item>
            </q-card-section>
            <q-card-actions align="right">
              <q-btn class="text-capitalize text-white" rounded color="cyan-8" icon="lock">
                Change Password</q-btn>
            </q-card-actions>
          </q-card>
        </div>
        <div class="col-lg-4 col-md-4 col-xs-12 col-sm-12">
          <q-card class="bg-blue-grey-9 card-bg text-white no-shadow" bordered>
            <q-card-section class="text-center bg-transparent">
              <q-avatar size="120px" class="shadow-10">
                <img src="../../public/icons/discount.png">
              </q-avatar>
              <div class="text-h6 q-mt-lg">My Deals</div>
            </q-card-section>
            <q-item-section>
              <q-card-actions align="center">
                <q-btn label="New deal" class="text-capitalize" rounded color="cyan-8" icon="add" style="max-width: 120px"
                  @click="choseedealtypedialog = true"></q-btn>
              </q-card-actions>
            </q-item-section>
            <q-dialog v-model="choseedealtypedialog">
                    <ChooseDealType></ChooseDealType>
            </q-dialog>
          </q-card>
        </div>
      </div>
    </q-page>
    <br>   <br>   <br>  <br>  <br>  <br>
  </div>
</template>
<script>
import { defineComponent, ref } from 'vue';
// import CreateStore from 'components/CreateStore.vue';
import ChooseDealType from 'src/components/ChooseDealType.vue';
import ChooseStoreType from 'src/components/ChooseStoreType.vue';
import axios from 'axios';

export default defineComponent({
  name: 'UserProfile',
  components: {
    // CreateStore,
    ChooseStoreType,
    ChooseDealType,
  },
  data: () => ({
    stores: [],
    user_name: '',
    email: '',
    first_name: '',
    last_name: '',
    address: '',
    city: '',
    post_code: '',
  }),
  setup() {
    return {
      user_details: {},
      password_dict: {},
      // createstordialog: ref(false),
      choosestoredialog: ref(false),
      choseedealtypedialog: ref(false),
    };
  },
  methods: {
    refreshStore() {
      const body = {};
      body.username = localStorage.getItem('user');
      console.log('body', body);
      axios.post('http://localhost:3000/store/getStore', body).then((response) => {
        this.stores = response.data;
        console.log('test', response.data);
      });
    },
    getPersonalData() {
      const body = {};
      body.username = localStorage.getItem('user');
      console.log('body', body);
      axios.get('http://localhost:3000/users/personalData', { params: { body } }).then((response) => {
        console.log('personalData', response.data);
        this.user_name = response.data[0].username;
        this.email = response.data[0].email;
        this.first_name = response.data[0].first_name;
        this.last_name = response.data[0].last_name;
        this.address = response.data[0].address;
        this.city = response.data[0].city;
        this.post_code = response.data[0].post_code;
      });
    },
    updatePersonalData() {
      const body = {};
      body.username = localStorage.getItem('user');
      body.email = this.email;
      body.first_name = this.first_name;
      body.last_name = this.last_name;
      body.address = this.address;
      body.city = this.city;
      body.post_code = this.post_code;
      console.log('body', body);
      axios.post('http://localhost:3000/users/updatePersonalData', body).then((response) => {
        console.log('getPersonalData', response.data);
        this.getPersonalData();
      });
    },
  },
  mounted() {
    this.refreshStore();
    this.getPersonalData();
    // console.log(this.deals);
  },
});
</script>
<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Carter+One&display=swap')

div
  font-family: 'Carter One', cursive

</style>
