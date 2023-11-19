<template>

<div>
        <h2 id="burgername">{{ burger.name }}</h2>

        <img :src="burger.imgUrl" alt="Burger image" style="width: 200px;">
        

           <p> <ul>
              <li> {{ burger.kCal +' '+ 'kCal'}}</li>
              <li><span v-html="burger.lactose ? 'Contains <strong>lactose</strong>' : '<strong>Lactose</strong> free'"></span></li>
  <li><span v-html="burger.gluten ? 'Contains <strong>gluten</strong>' : '<strong>Gluten</strong> free'"></span></li>
            </ul> </p>
            <div id="amountordered">Amount ordered: {{ amountOrdered }} <br>
            <button v-on:click="increaseAmount">Increase</button>
            <button v-on:click="decreaseAmount">Decrease</button>
        </div></div>
    
</template>
  
<script>
export default {
  props: {
    burger: {
      type: Object,
      required: true,
    },
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    increaseAmount: function () {
      this.amountOrdered++;
      this.$emit('updateOrder', { burgerName: this.burger.name, amount: this.amountOrdered });
    },
    decreaseAmount: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('updateOrder', { burgerName: this.burger.name, amount: this.amountOrdered });
        
        
      }
    }
  }
};
</script>
 
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
 #amountordered {
  text-align: center;
  }</style>
  