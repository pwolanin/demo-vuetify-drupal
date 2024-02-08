<template>
  <h1>Data from JSON:API Demo</h1>
  <v-container class="fill-height">

    <v-data-table-server
    v-model:items-per-page="itemsPerPage"
    :search="search"
    :headers="headers"
    :items-length="totalItems"
    :items="serverItems"
    :loading="loading"
    items-per-page="25"
    :items-per-page-options="[25, 50]"
    item-value="name"
    @update:options="loadItems"
  >
  <template #top="topProps">
    <div>{{  topProps.itemsPerPage }} Items per page.</div>
    <div><v-text-field size="20" v-model="name" hide-details placeholder="Search name..." ></v-text-field></div>
  </template>
</v-data-table-server>

    <v-responsive class="align-center text-center fill-height">
      <v-img height="100" src="@/assets/logo.svg" />

      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

      <h2 class="text-h2 font-weight-bold">Vuetify</h2>

      <div class="py-14" />

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn
            href="https://vuetifyjs.com/components/all/"
            min-width="164"
            rel="noopener noreferrer"
            target="_blank"
            variant="text"
          >
            <v-icon
              icon="mdi-view-dashboard"
              size="large"
              start
            />

            Components
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            color="primary"
            href="https://vuetifyjs.com/introduction/why-vuetify/#feature-guides"
            min-width="228"
            rel="noopener noreferrer"
            size="x-large"
            target="_blank"
            variant="flat"
          >
            <v-icon
              icon="mdi-speedometer"
              size="large"
              start
            />

            Get Started
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            href="https://community.vuetifyjs.com/"
            min-width="164"
            rel="noopener noreferrer"
            target="_blank"
            variant="text"
          >
            <v-icon
              icon="mdi-account-group"
              size="large"
              start
            />

            Community
          </v-btn>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script>

import { VDataTable, VDataTableFooter, VDataTableServer } from 'vuetify/components/VDataTable';
import axios from "axios";
import { Jsona } from 'jsona';

// You would normally import some config to set this.
const baseUrl = 'http://localhost:8888';

const jsonapiRequest = axios.create({
  baseURL: baseUrl + "/jsonapi",
  headers: {
    "Content-Type": "application/vnd.api+json",
  },
});

jsonapiRequest.interceptors.response.use((response) => {
  if (response.data?.data) {
    const dataFormatter = new Jsona();
    response.data.deserialized = dataFormatter.deserialize(response.data);
  }
  return response;
});

const JsonAPI  = {
  async fetch ({ page, itemsPerPage, sortBy, search }) {
    const queries = {
      page: {},
    };
    queries.page.limit = itemsPerPage;
    if (page > 1) {
      queries.page.offset = itemsPerPage * (page - 1);
    }
    if (sortBy.length) {
      let {key, order} = sortBy[0];
      queries.sort = (order === 'desc' ? '-' : '') + key;
    }
    else {
      queries.sort = 'field_last_name';
    }
    if (search.name) {
      queries.filter = {};
      queries.filter.nameSearch = {};
      queries.filter.nameSearch.condition = {};
      queries.filter.nameSearch.condition.path = 'title';
      queries.filter.nameSearch.condition.value = search.name;
      queries.filter.nameSearch.condition.operator = "CONTAINS";
    }
    return jsonapiRequest.get('node/contact', { params: queries });
  }
};

  export default {
    data: () => ({
      itemsPerPage: 25,
      headers: [
        { title: 'First Name', key: 'field_first_name', align: 'end' },
        { title: 'Last Name', key: 'field_last_name', align: 'end' },
        { title: 'Address', key: 'field_contact_address', align: 'end' },
      ],
      serverItems: [],
      loading: true,
      totalItems: 0,
      name: '',
      search: '',
    }),
    watch: {
      name () {
        this.search = String(Date.now())
      },
    },
    methods: {
      loadItems ({ page, itemsPerPage, sortBy }) {
        this.loading = true
        JsonAPI.fetch({ page, itemsPerPage, sortBy, search: { name: this.name } }).then((response) => {
          this.serverItems = response.data.deserialized;
          this.totalItems = response.data.meta.count;
          this.loading = false
        })
      },
    },
  }
</script>

<style scoped>
h1  {
  text-align: center;
}
</style>