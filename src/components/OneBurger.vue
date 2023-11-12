<template>

<div>
        <h2 id="burgername">{{ burger.name }}</h2>

        <img :src="burger.imgUrl" alt="Burger image" style="width: 200px;">
        

           <p> <ul>
              <li> {{ burger.kCal +' '+ 'kCal'}}</li>
              <li>Contains <span>{{ burger.containsLactose ? 'lactose' : 'no lactose' }}</span></li>
              <li>Contains <span>{{ burger.containsGluten ? 'gluten' : 'no gluten' }}</span></li>
            </ul> </p>
        </div>
    
</template>
  
<script>
export default {
  props: {
    burger:{
      type: Object,
    required: true
  }
},
mounted() {
    console.log(this.burger);
  }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
  