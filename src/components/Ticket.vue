<template>
  <div class='ticket'>
    <div class='left'>
      <div class='date'>{{unixDateString(ticket.departure_time)}}</div>
      <div class='image'>
        <img v-bind:src='transferLogoUrl(ticket.carrier)'>
      </div>
    </div>
    <div class='middle'>
      <div class='part'>
        <div class='air'>{{ticket.departure_airport}}</div>
        <div class='time'>{{unixTimesStampToTimesString(ticket.departure_time)}}</div>
      </div>
      <div class='part'>
        <div class='travel'>{{minutesToDuratiomString(ticket.flight_duration_minutes)}}</div>
        <hr>
        <div class='kind'>{{prettyKind(ticket.kind)}}</div>
      </div>
        <img src='image/black-plane.png'/>
      <div class='part'>
        <div class='air'>{{ticket.arrival_airport}}</div>
        <div class='time'>{{unixTimesStampToTimesString(ticket.departure_time + ticket.flight_duration_minutes * 60)}}</div>
      </div>
    </div>
    <div class='right'>
      <div class='price'>{{ticket.price}}</div>
      <div id='add-button' class='button'>Купить</div>
    </div>
    <div class="delete-edit">
      <div class="delete" v-on:click='deleteTicket(ticket.id)'>
        <img src='image/remove.png'/>
      </div>
      <div class="edit" v-on:click='editTicket(ticket.id)'>
        <img src='image/tools.png'/>
      </div>
    </div>
  </div>
</template>

<script>
 export default {
   name: "Ticket",
   props:['ticket'],
   methods:{
     fetchAiportName: function(airport_code){
       switch (airport_code) {
          case 'vko':
            return 'Внуково'
          case 'svo':
            return 'Шереметьево'
          case 'dme':
            return 'Домодедово'
         default:
            return 'Любой'
       }
     },
     minutesToDuratiomString: function(duration_minutes){
       var hours = Math.trunc(duration_minutes / 60)
       var minutes = duration_minutes - hours * 60
       return `${hours}ч ${minutes}м`
     },
     unixDateString: function(unixdate){
       var date = new Date (unixdate * 1000)
       return `${date.getDay()}-${date.getMonth()}-${date.getFullYear()}`
     },
     unixTimesStampToTimesString: function(unixtime){
       var date = new Date(unixtime * 1000)
       return `${date.getHours()}:${date.getMinutes()}`
     },
     prettyKind: function(kind){
       var result
       if(kind == 'docking'){
          result = 'Стыковочный'
       } else {
         result = 'Прямой'
       }
       return result
     },
     editTicket: function(index){
        console.log("edit City with index:",index)
        this.showForm = true
        var ticket = this.tickets[index]
        this.form_ticket_departure_airport = ticket.departure_airport
        this.form_ticket_departure_time = ticket.departure_time
        this.form_ticket_arrival_airport = ticket.arrival_airport
        this.form_ticket_flight_duration_minutes = ticket.flight_duration_minutes
        this.form_ticket_price = ticket.price
        this.form_ticket_carrier = ticket.carrier

        this.editing_ticket_index = index
      },
     deleteTicket: function(id){
        // this.tickets.splice(index,1)
        console.log('delete item with id:', id)
        this.$emit('delete-item',id)
      },
     transferLogoUrl: function(carrier){
        var url;
        // console.log('transferLogoUrl', carrier)
        switch(carrier) {
          case 'yral':
            url = 'image/ural.jpg';
            break;
          case 'pobeda':
            url = 'image/vin.png';
            break;
          case 'aeroflot':
            url = 'image/aeroflot_ru.jpg';
            break;
            default:
          }
        // console.log('transferLogoUrl result:',url)
        return url
      },
     addButtonClicked: function(){
        console.log('Yo!')
        this.showForm = true
      },
     saveButtonClicked: function(){
      var ticket = {
        departure_airport: this.form_ticket_departure_airport,
        departure_time: this.form_ticket_departure_time,
        arrival_airport: this.form_ticket_arrival_airport,
        carrier: this.form_ticket_carrier,
        price: this.form_ticket_price,
        flight_duration_minutes: parseInt(this.form_ticket_flight_duration_minutes)
      }

      if(this.editing_ticket_index != null){
          this.tickets.splice(this.editing_ticket_index,1,ticket)
          this.editing_ticket_index = null
      } else {
        this.tickets.unshift(ticket)
      }
      this.showForm = false
      this.clearForm()
     },
     cancelButtonClicked: function(){
      this.showForm = false
        this.clearForm()
     },
     clearForm: function(){
        this.form_ticket_departure_air = null,
        this.form_ticket_departure_time = null,
        this.form_ticket_price = null,
        this.editing_city_index = null
      }
   },
 }
</script>


<style>
  .ticket > div{
    display: inline-block;
    padding: 0;
    margin: 0;
  }
  .ticket{
    border: 1px solid rgba(0,0,0,0.5);
    border-radius: 12px;
    box-shadow: 0 0 10px rgba(0,0,0,0.5);
    margin: 20px;
    padding: 10px;
  }

  .ticket > .left {
    width: 25%;
  }
  .ticket > .middle {
    width: 50%;
  }
  .ticket > .middle > .part {
    display: inline-block;
  }
  .ticket > .right {
    width: 15%;
  }
  .ticket .image img{
    width: 75px;
    height: 36px;
    left: 58px;
    top: 88px;
  }
  .button {
    background: green;
    color: white;
    max-width: fit-content;
    padding: 3px;
    margin-right: 10px;
  }

  .price {
    height: 38px;
    font-weight: 700;
    font-size: 20px;
  }
  .controls {

  }
  .contr .button {
    background: blue;
    color: white;
    max-width: fit-content;
    font-weight: 700;
    font-size: 15px;
    margin-top: 5px;
    border-radius: 10px;
    padding: 5px;
  }
  .blue {
    background-color: blue;
  }

  .gray {
    background-color: gray;
  }

  .inline {
    display: inline-block;
    margin: 10px;
    padding: 10px;
  }
  .delete {
   margin: 5px;
   display: inline-block;
  }

  .delete-edit {
    float: right;
  }
  .edit{
    display: inline-block;
  }
   img {
    width: 15px;
  }
  .kind {
    color:#00733E;
    font-weight: 400;
    font-size: 15px;
    align-content: center;
  }

</style>
