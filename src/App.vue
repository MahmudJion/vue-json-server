<template>
  <div id="app" class="container">
    <h1>Shipments</h1>
    <div class="wrapper">
      <input type="text" v-model="search" placeholder="Search" />
    </div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Name</th>
        </tr>
      </thead>
      <tbody v-for="(shipment, index) of ShipmentsFilteredList" :key="index">
        <tr>
          <th scope="row">{{ shipment.id }}</th>
          <td>{{ shipment.name }}</td>
        </tr>
      </tbody>
    </table>
    <div v-if="shipments.length != 24">
      <button
        v-if="shipments.length > 4"
        type="button"
        class="btn btn-primary"
        @click.prevent="getNextPosts()"
      >
        Load More
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      search: "",
      shipments: [],
    };
  },
  async mounted() {
    try {
      const res = await axios.get(`http://localhost:3000/shipments`);
      this.shipments = res.data.slice(0, 20);
    } catch (error) {
      console.error(error);
    }
  },
  computed: {
    ShipmentsFilteredList() {
      return this.shipments.filter((shipment) =>
        shipment.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
  methods: {
    async getNextPosts() {
      try {
        const res = await axios.get("http://localhost:3000/shipments", {
          params: {
            _start: this.shipments.length,
            _limit: 20,
          },
        });
        this.shipments = [...this.shipments, ...res.data];
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>


<style></style>
