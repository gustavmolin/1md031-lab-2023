<template>
    
    <div id="orders">
      <div id="orderList">
        <button v-on:click="clearQueue">Clear Queue</button>
        <div v-for="(order, key) in orders" :key="key" id = "ordernumber">
   Order #{{ key }} 
   <div v-for="(amount, burgerName) in order.orderItems" :key="burgerName" >
  {{ burgerName }}: {{ amount }}

</div>
 <div id="customerinfo" v-for="item in order.customerInfo" :key="item">
      {{ item }}
    </div>
    
        </div>
    </div>
    <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}
  </div>
</div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: {}
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      }
    }
  }
  </script>
  <style>

 

#customerinfo {
  font-style: italic;
  
}


  #orderList {
    top:1em;
  left:1em;
  position: absolute;
  z-index: 2;
  color:black;
  background: rgba(255,255,255, 0.5);
  padding: 1em;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-gap: 2em;
  max-height: 80%;
  overflow-y: auto;
  

    

    
  }
  #ordernumber {
    border-top: 1px solid black;;
    padding: 1em;
    
  }
  
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  </style>
  