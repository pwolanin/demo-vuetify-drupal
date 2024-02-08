<template>
  <v-container class="fill-height">

    <v-data-table-server
    v-model:items-per-page="itemsPerPage"
    :search="search"
    :headers="headers"
    :items-length="totalItems"
    :items="serverItems"
    :loading="loading"
    item-value="name"
    @update:options="loadItems"
  ></v-data-table-server>

    <v-responsive class="align-center text-center fill-height">
      <v-img height="100" src="@/assets/logo.svg" />

      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

      <h1 class="text-h2 font-weight-bold">Vuetify</h1>

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
  baseURL: config.baseUrl + "/jsonapi",
  headers: {
    ...defaults.headers,
    "Content-Type": "application/vnd.api+json",
  },
});

jsonapiRequest.interceptors.response.use((response) => {
  if (response.data?.data) {
    dataFormatter = new Jsona();
    response.data.deserialized = dataFormatter.deserialize(response.data);
  }
  return response;
});

const JsonAPI  = {
  async fetch ({ page, itemsPerPage, sortBy, search }) {
    const queries = {};
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
      calories: '',
      search: '',
    }),
    watch: {
      name () {
        this.search = String(Date.now())
      },
      calories () {
        this.search = String(Date.now())
      },
    },
    methods: {
      loadItems ({ page, itemsPerPage, sortBy }) {
        this.loading = true
        JsonAPI.fetch({ page, itemsPerPage, sortBy, search: { name: this.name, calories: this.calories } }).then((response) => {
          console.log(response);
          this.serverItems = items
          this.totalItems = total
          this.loading = false
        })
      },
    },
  }
</script>