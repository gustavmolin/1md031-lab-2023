<template>
  <head>
    <link rel="stylesheet" type="text/css" href="style.css">
    <link rel="resetsheet" type="text/css" href="reset.css">
    <title>Hamburgarstugan</title>
    <meta charset="utf-8" />
  </head>

  <div id="wholeheader">
    <img src="https://klimatkommunerna.se/wp-content/uploads/2021/05/uppsala-kommun-scaled.jpg" id="headpicture">
    <header>
      <h1>Welcome to Hamburgarstugan!</h1>

    </header>


  </div>
  <main>
    <section id="burgerwindow">
      <Burger v-for="burger in menu" :key="burger.name" :burger="burger" @updateOrder="updateOrder" />
    </section>



    <section id="orderinfo">

      <h4>About us:</h4>We are a local hamburger restaurant located in Uppsala. Welcome!


      <form>
        <fieldset>
          <p>
            <label for="fullname">Your name</label><br>
            <input type="text" id="fullname" name="fn" required="required" placeholder="First- and Last name"
              v-model="name">
          </p>

          <p>
            <label for="email">E-mail</label><br>
            <input type="text" id="email" name="em" required="required" placeholder="E-mailaddress" v-model="email">
          </p>
          <p>
            <label for="payment">Choose payment method</label>

            <select id="payment" name="pay" v-model="paymentMethod">
              <option>Card</option>
              <option>Swish</option>
              <option>Invoice</option>
              <option>Cash</option>
            </select>
          </p>
          <h3>Please indicate point of delivery:</h3>
          <div class="map-container">
            <div class="map" @click="setLocation($event)">
              <div class="target" v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">T</div>
            </div>
          </div>


          <div>
            <legend>Gender:</legend>
            <div>
              <input type="radio" id="Male" name="drone" value="Male" v-model="gender" />
              <label for="Male">Male</label>
            </div>

            <div>
              <input type="radio" id="Female" name="drone" value="Female" v-model="gender" />
              <label for="Female">Female</label>
            </div>

            <div>
              <input type="radio" id="Do not wish to provide" name="drone" value="Do not wish to provide"
                v-model="gender" />
              <label for="Do not wish to provide">Do not wish to provide</label>
            </div>
          </div>
        </fieldset>
      </form>
    </section>
    <br>

    <button @click="placeOrder">Place order
      <img
        src="https://img.freepik.com/premium-vector/free-shipping-sticker-free-delivery-badge-with-truck-vector-isolated-background-eps-10_399089-2425.jpg?w=740"
        style="width: 30px;">
    </button>

  </main>

  <footer>
    <hr> Hamburgarstugan AB 2023 &copy;
    <hr>
  </footer>
</template>



<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();


//Define Menuitem cunstructor function



/*function MenuItem(name, imgUrl, kCal, containsGluten, containsLactose) {
  this.name = name;
  this.imgUrl = imgUrl;
  this.kCal = kCal;
  this.containsGluten = containsGluten;
  this.containsLactose = containsLactose;
}
const burgerarray = [
  new MenuItem("Mr Bacon", "https://img.freepik.com/free-photo/craft-beef-burger-french-fries-wooden-table-melting-chesses-isolated-black-background-ai-generative_157027-1745.jpg?w=1380&t=st=1699200430~exp=1699201030~hmac=a6f742dfe670165249fcd12c152a7fde43a55e42b222489fcf30961ceb89b39f", "250", false, false),
  new MenuItem("Mr Cheese", "https://img.freepik.com/free-photo/grilled-beef-burger-with-cheese-tomato-generated-by-ai_24640-82315.jpg?t=st=1699197258~exp=1699200858~hmac=7fe225706fe8da5296e4673cc461c48e76d6579bc7f5ac43a8a71b00cf6b8f36&w=1380", "450", false, true),
  new MenuItem("Mr Chicken", "https://img.freepik.com/free-photo/burger-with-mayonnaise-sauce-it_188544-15976.jpg?t=st=1699200117~exp=1699203717~hmac=2661f7759652976bc37fa0d93b78c1dbcedab8c29cdc5779a4cf78f7865ee2b6&w=1380", "850", true, false)
];
*/
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      menu,
      location: {
        x: 0,
        y: 0,
      },
      orderedBurgers: {},
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    setLocation: function (event) {

      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };

      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      };
    },

    placeOrder: function () {
      console.log(this.orderedBurgers)
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: this.location,
        orderItems: this.orderedBurgers,
        customerInfo: {
          name: this.name,
          email: this.email,
          gender: this.gender,
          paymentMethod: this.paymentMethod,
        }
      });
    },
    updateOrder: function (order) {
      if (order.amount > 0) {
        this.orderedBurgers[order.burgerName] = order.amount;
      } else {
        delete this.orderedBurgers[order.burgerName];
      }
    },

  },
}



</script>

<style>
h1 {
  color: black;
  margin-right: 30px;
  /*font-size: 5em;*/
  font-family: arial;


}



.main,
header,
footer,
nav ul {
  max-width: 80rem;
  margin: 0 auto 0 auto;
}




header h1 {
  position: relative;
  text-align: center;
  margin-top: -48%;


}

@media screen and (min-width: 1920px) {
  header h1 {
    margin-top: -90%;
  }
}

#wholeheader {
  margin-left: 10px;
  margin-right: 10px;
  position: relative;
  overflow: hidden;
  height: 220px;
  width: 100%;
  margin-bottom: 10px;
}

#headpicture {
  opacity: 0.65;
  width: 100%;
  height: auto;


}

#orderinfo {
   border: 4px dotted black;
   margin-left: 10px;
}
section {
  padding: 5px;
  margin: 6px;
}


button:hover {
  background-color: gray;
  transition: 0.7s;
  cursor: pointer;
}

#burgername {
  margin-left: 40px;
}

#burgerwindow {

  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  justify-items: center;
  align-items: center;
  background-color: #000000;
  width: 100%;
  margin: auto;
  border: 4px dotted white;
  color: white;
  padding: 10px;
  margin-left: 10px;
}



.map-container {
  width: 70%;
  height: 700px;
  overflow: scroll;
}

.map {
  height: 1078px;
  width: 1920px;
  background: url("/public/img/polacks.jpg");
  background-repeat: no-repeat;
  cursor: crosshair;
  text-align: center;
  position: relative;
}

.target {
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: red;
  border-radius: 50%;
  text-align: center;
}
</style>
