<template>
  <v-app>
    <v-container fluid>
      <v-layout justify-space-between row wrap>
        <v-flex lg2>
          <p>With Selected :</p>
        </v-flex>
        <v-flex lg2>
          <v-overflow-btn
            :items="statusActions"
            outlined
            label="Status Actions"
          ></v-overflow-btn>
        </v-flex>
        <v-flex lg2>
          <v-overflow-btn
            :items="logActions"
            outlined
            label="Log Actions"
          ></v-overflow-btn>
        </v-flex>
        <v-flex lg2>
          <v-btn color="red" dark x-large @click="deleteAction()">Delete Selected</v-btn>
        </v-flex>
        <v-flex></v-flex>
      </v-layout>
      <v-data-table
        v-model="selected"
        :headers="contentHeaders"
        :items="content"
        :single-expand="singleExpand"
        :expanded.sync="expanded"
        @item-expanded="loadDetails"
        :single-select="singleSelect"
        show-select
        item-key="name"
        show-expand
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Expandable Table</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-switch
              v-model="singleExpand"
              label="Single expand"
              class="mt-2"
            ></v-switch>
          </v-toolbar>

        </template>
        <template v-slot:expanded-item="{ headers, item }">
          <td :colspan="headers.length">
           <v-simple-table>
              <tbody>
                <tr>
                  <td>Logged :</td>
                  <td><v-btn small rounded color="primary" dark>{{ item.summary.logged }}</v-btn></td>
                </tr>
                <tr>
                  <td>State :</td>
                  <td><v-btn small rounded color="primary" dark>{{ item.summary.state }}</v-btn></td>
                </tr>
                <tr>
                  <td>Status :</td>
                  <td><v-btn small rounded color="success" dark>{{ item.summary.status }}</v-btn></td>
                </tr>
              </tbody>
           </v-simple-table>
          </td>
        </template>
      </v-data-table>
    </v-container>
  </v-app>
</template>

<script lang="ts">
/* eslint-disable */

import Vue from 'vue';

declare interface Content {
  sn: string;
  model: string;
  name: string;
  mac: string;
  sammyVer: string;
  firmwareVer: string;
  registered: string;
  user: string;
  logLimit: string;
  summary: any;
}

export default Vue.extend({
  name: 'App',

  data: () => ({
    expanded: [],
    selected: [] as Content[],
    statusActions: ['Update Status', 'Delete Data'],
    logActions: ['Update Log', 'Delete Log'],
    singleSelect: false,
    singleExpand: true,
    contentHeaders: [
      {
        text: 'SN',
        align: 'start',
        sortable: false,
        value: 'sn'
      },
      { text: 'Model', value: 'model' },
      { text: 'Name', value: 'name' },
      { text: 'MAC', value: 'mac' },
      { text: 'Sammy Ver.', value: 'sammyVer' },
      { text: 'Firmware Ver.', value: 'firmwareVer' },
      { text: 'Registered', value: 'registered' },
      { text: 'User', value: 'user' },
      { text: 'Log Limit', value: 'logLimit' },
      // { text: 'Action', value: 'action' },
    ],
    content: [] as Content[],
  }),
  created() {

    const apiData: any[] = [
      { sn: '55098127423490', model: 'SAM-SMR01', name: 'Smart Relay', mac: '', sammyVer: '1.0.0', firmwareVer: 'WDB01/1.00', registered: '11/17/2020, 2:07:49 PM', user: '', logLimit: '0 B', summary: ''},
      { sn: '55098127423491', model: 'SAM-SMR01', name: 'Silly Relay', mac: '', sammyVer: '1.0.0', firmwareVer: 'WDB01/1.00', registered: '11/17/2020, 2:07:49 PM', user: '', logLimit: '0 B', summary: ''},
      { sn: '55098127423492', model: 'SAM-SMR01', name: 'Genius Relay', mac: '', sammyVer: '1.0.0', firmwareVer: 'WDB01/1.00', registered: '11/17/2020, 2:07:49 PM', user: '', logLimit: '0 B', summary: ''},
    ];

    this.content = [...apiData];
  },
  methods: {
    loadDetails({item}: any) {

      // get from http / api
      const APIConsume = {logged: 'Yes', state: 'Initializing', status: 'Activated'}

      console.log(item)

      item.summary = APIConsume;

    },
    deleteAction() {
      let tempData :any = [] as Content[];
      tempData = [...this.content];
      
      let newData :any = [] as Content[];

      let indexData :number[] = [];

      this.selected.forEach((res: Content) => {
        tempData.forEach((dt: Content, index: number) => {
          if(dt.sn == res.sn) {
            indexData.push(index);
          }
        })
      });

      console.log(indexData);

      indexData = [...new Set(indexData)];

      console.log(indexData);

      for(let i = indexData.length - 1; i >= 0; i--) {
        tempData.splice(indexData[i], 1);
      }

      newData = [...tempData];

      // this.selected.forEach((res :Content) => {
      //     return tempData.filter((dt :Content) => {res.sn != dt.sn});
      // });

      this.content = newData;

      console.log(newData);
    }
  }
});
</script>
