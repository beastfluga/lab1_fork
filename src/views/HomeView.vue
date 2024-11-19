<template>
<div>
    <div>
    <h1>Burgers</h1>
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
    click here
    </div>
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");
//object constructor function
function MenuItem(pName, imgUrl, kcal, isGluten, isLactose) {
  this.productName = pName;
  this.imgageUrl = imgUrl;
  this.kcal = kcal;
  this.isGluten = isGluten;
  this.isLactose = isLactose;
}
const burgersList = [
  new MenuItem("Burger1", "url1", 300, false, false),
  new MenuItem("Burger2", "url2", 400, true, true),
  new MenuItem("Burger3", "url3", 7000, true, true),
];

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850}
               ]
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
console.log(burgersList);
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
