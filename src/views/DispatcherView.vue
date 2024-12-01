<template>
  <h2>The exact location of the order is at the bottom left of the T, the color of the T matches that of the order</h2>
    <div id="orders">
      <div id="orderList">
        <button v-on:click="clearQueue">Clear Queue</button>
        <div id="eachOrderList" v-for="(order, key) in orders" v-bind:key="'order'+key" v-bind:style="{color: order.orderColor}">
          
          #{{ key }}: <br>
          <b>Personal info:</b>
          <div v-for="(value, info) in order.orderInfo" v-bind:key="'info'+value">
            {{ info }}: {{ value }}
          </div>
          
          <b>Burger info:</b>
          <div v-for="(count, burger) in order.orderItems" v-bind:key="'count'+burger">
            {{ burger }}: {{ count }}
          </div>
          

          <hr>
        </div>
        
      </div>
      <div id="dots">
        
         <div v-for="(order, key) in orders" 
          v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px',  color: order.orderColor}"
           v-bind:key="'dots' + key" >
           <b>T</b> 
          </div> 
          
         
      </div>
      orders:,{{ orders }}
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io("localhost:3000");
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null,
       
      }

    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      changeStatus: function(orderId) {
        socket.emit('changeStatus', {orderId: orderId, status: "Annan status"});

      }
    }
  }
  </script>
  <style>
  #orderList {
    top:5em;
    left:73em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
    width: 400px;
    height: 800px;
    overflow: scroll;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    background-image: url('/img/polacks.jpg');
  }
  
  #dots div {
    position: absolute;
    
    
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

  </style>
  