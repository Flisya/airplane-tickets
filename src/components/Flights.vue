<template>
  <div align="center">
    <h1>Hello Flights!</h1>
    <div class="tickets">
    <Ticket v-for="ticket in tickets" v-bind:ticket="tickets[0]"></Ticket>
  </div>
  </div>
</template>

<script>
  import Ticket from './Ticket.vue'


  export default {
    name:'Flights',
    components: { Ticket },
    data: function() {
      return {
        message: 'Привет, Vue!',
        showForm: false,
        tickets: [
          {id: 1, departure_airport:'vko', departure_time:1576686396, flight_duration_minutes: 160, arrival_airport:'dme',  carrier:'yral', price:3900, kind:'docking'},
          {id: 4244324, departure_airport:'dme', departure_time:1577885196, flight_duration_minutes: 110, arrival_airport:'vko', carrier:'pobeda', price:2400, kind:'straight'},
          {id: 4564, departure_airport:'dme', departure_time:1577885196, flight_duration_minutes: 110, arrival_airport:'vko', carrier:'yral', price:2400, kind:'docking'},
          {id: 2, departure_airport:'svo', departure_time:1577885196, flight_duration_minutes: 150, arrival_airport:'dme', carrier:'aeroflot', price:2400, kind:'docking'},
          {id: 453, departure_airport:'svo', departure_time:1577885196, flight_duration_minutes: 134, arrival_airport:'svo', carrier:'yral', price:2400, kind:'straight'},
          {id: 99, departure_airport:'dme', departure_time:1578166000, flight_duration_minutes: 90, arrival_airport:'svo',  carrier:'aeroflot', price:2500, kind:'docking'}
        ],
        form_ticket_departure_airport:null,
        form_ticket_departure_time:null,
        form_ticket_arrival_airport:null,
        form_ticket_flight_duration_minutes:null,
        form_ticket_price:null,
        form_ticket_carrier:null,
        editing_ticket_index:null,
        filter_depature_airport:'',
        filter_arrival_airport:''
      }
    },
  }
</script>

<style>
  .tickets {
    padding-top: 20px;
    width: 600 px;
  }

</style>
