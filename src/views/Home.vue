<template>
  <LoadingComponent/>
  <div class="container">
    <h1>Welcome</h1>

    <div class="card">
      <h1>Liste des tests</h1>
      <button type="button" @click="getTestList">Rafraîchir</button>

      <p v-for="(test, key) in testList ">
        {{test.content}}
      </p>
    </div>


  </div>
</template>

<script>

import { v4 as uuidv4 } from 'uuid';
import {mapActions, mapGetters} from "vuex";
import LoadingComponent from "@/components/util/LoadingComponent.vue";
import TestApiService from "@/services/api/testApiService.js";
import ErrorService from "@/services/errorService.js";
export default {
  name: 'Home',
  components: {
    LoadingComponent

  },
  data() {
    return {
      testList :[],
    }
  },
  computed: {
    ...mapGetters(['isTestMode']),
  },
  methods: {
    ...mapActions(['setLoading']),
      async getTestList(){
        await this.setLoading(true);
        await TestApiService.getAll().then((response) => {
          this.testList = response.data;
        }).catch((error) => {
          ErrorService.showErrorInAlert(error);
        });
        await this.setLoading(false);
      }
  },
  mounted() {
  }
}

</script>