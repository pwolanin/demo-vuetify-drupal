<template>
  <v-container class="fill-height">
    Hello?
    <v-data-table
    :items="items" 
    :headers="headers"
    ></v-data-table>
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

<script setup>

import { VDataTable } from 'vuetify/components/VDataTable';

const headers = [
    { title: 'Pyramid', value: 'name' },
    { title: 'Location', value: 'location' },
    { title: 'Construction Date', value: 'constructionDate' },
    {
      title: 'Dimensions',
      align: 'center',
      children: [
        { title: 'Height (m)', value: 'height' },
        { title: 'Base (m)', value: 'base' },
        { title: 'Volume (m³)', value: 'volume' },
      ],
    },
  ]

  const items = [
    {
      name: 'Great Pyramid of Giza',
      location: 'Egypt',
      height: '146.6',
      base: '230.4',
      volume: '2583285',
      constructionDate: 'c. 2580–2560 BC',
    },
    {
      name: 'Pyramid of Khafre',
      location: 'Egypt',
      height: '136.4',
      base: '215.3',
      volume: '1477485',
      constructionDate: 'c. 2570 BC',
    },
    {
      name: 'Red Pyramid',
      location: 'Egypt',
      height: '104',
      base: '220',
      volume: '1602895',
      constructionDate: 'c. 2590 BC',
    },
    {
      name: 'Bent Pyramid',
      location: 'Egypt',
      height: '101.1',
      base: '188.6',
      volume: '1200690',
      constructionDate: 'c. 2600 BC',
    },
    {
      name: 'Pyramid of the Sun',
      location: 'Mexico',
      height: '65',
      base: '225',
      volume: '1237097',
      constructionDate: 'c. 200 CE',
    },
  ]
  //
</script>
