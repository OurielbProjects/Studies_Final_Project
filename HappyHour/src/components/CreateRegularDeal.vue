<!-- eslint-disable max-len -->
<template>
  <div class="column q-pa-lg">
    <div class="row">
      <q-card square class="shadow-24" style="width:300px;height:485px;">
        <q-card-section class="bg-blue-grey-9">
          <h4 class="text-h5 text-white q-my-md">create store</h4>
        </q-card-section>
        <q-card-section>
          <q-form class="q-px-sm q-pt-xl">
            <q-input square clearable v-model="storename" type="text" label="Store name">
              <template v-slot:prepend>
                <q-icon name="email" />
              </template>
            </q-input>
          </q-form>
        </q-card-section>
        <q-card-actions class="q-px-lg">
          <q-btn unelevated size="lg" color="cyan-8" class="full-width text-white" label="Create" @click="createDeal" />
        </q-card-actions>
        <q-card-section class="text-center q-pa-sm">
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'RegistraTion',

  props: {
    goLogIn:
    {
      type: Function,
      default() { return console.log('Default function aa'); },
    },
  },
  data() {
    return {
      storename: '',
    };
  },
  methods: {
    createDeal() {
      const newStor = {};
      newStor.storename = this.storename;
      newStor.username = localStorage.getItem('user');
      axios.post('http://localhost:3000/store/create', newStor).then((response) => {
        alert('You have successfully registered');
      });
      return true;
    },
  },
};
</script>
