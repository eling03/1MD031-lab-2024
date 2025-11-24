<template>
  <div id="orders">

    <div id="orderList">
      <div
        v-for="(order, key) in orders"
        :key="'order' + key"
        class="orderrow"
      >
        <strong>#{{ key }}</strong>
        <span> — {{ formatItems(order.orderItems) }}</span>
        <div v-if="order.customer" class="customer">
          {{ order.customer.name }} , {{ order.customer.email }} ,
          {{ order.customer.payment }} , {{ order.customer.gender }}
        </div>
      </div>

      <button @click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots">
      <div
        v-for="(order, key) in orders"
        :key="'dot' + key"
        class="dot"
        :style="{ left: order.details.x+ 'px', top: order.details.y + 'px' }"
      >
         {{ order.orderId}}
      </div>
    </div>
  </div>
</template>
  <script>
  import io from 'socket.io-client'
  const socket = io("localhost:3000");
  
  export default {
    name: 'DispatcherView',
    data() {
      return { orders: {} };
    },
    created() {
      socket.on('currentQueue', data => {
        console.log('currentQueue', data);
        this.orders = data.orders || {};
      });
    },
    methods: {
      formatItems(obj) {
        if (!obj) return '';
        return Object.entries(obj)
          .map(([name, amount]) => `${name} (${amount})`)
          .join(', ');
      },
      clearQueue() {
          socket.emit('clearQueue');
          this.orders = {};
        }
    }
    }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
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
    background-image: url('/img/polacks.jpg');
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    padding-right: 40px;
  }


  </style>
  