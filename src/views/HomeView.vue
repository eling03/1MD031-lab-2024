

<template>
<header>
    <img id="rubrikbild" src="/img/eatburger.avif" alt="Header image">
        <h1>Välkommen till Nom Nom Burgers!</h1>
</header>
<main>
<section id="burgare-info">
        <div class="wrapper">
          <Burger v-for="burger in burgers"
                  :burger="burger"
                  :key="burger.name"
                  @orderedBurger="addToOrder"
                  />
    </div>
</section>

<section><h3>Fyll i uppgifter nedan för online-beställning!</h3>
Maten är redo inom ca 20 minuter från beställning.
<p>
    <label for="name">Fullständigt namn</label><br>
    <input type="text" id="name" name="fn" required="required" v-model="orderForm.name" placeholder="För- och efternamn">
</p>
<p>
    <label for="email">E-mail</label><br>
    <input type="text" id="email" name="em" required="required" v-model="orderForm.email" placeholder="E-mail">
</p>
<p>
    <label for="Gata">Gatuadress</label><br>
    <input type="text" id="Gata" name="em" required="required" v-model="orderForm.street" placeholder="Gata">
</p>
<p>
    <label for="Gatunummer">Gatunummer</label><br>
    <input type="number" id="Gatunummer" name="em" required="required" v-model="orderForm.number" placeholder="Nummer">
</p>
<div id="kartobjektet">
    <div id="map" v-on:click="addOrder">
</div>
</div>
<p>
    <label for="recipient">Betalsätt</label><br>
    <select id="recipient" v-model="orderForm.payment" name="rcp">
        <option selected="selected">Swish</option>
        <option>Bankkort online</option>
        <option>Betala på plats (kort)</option>
        <option>Betala på plats (kontanter)</option>
    </select>
</p>
<p>
    <label for="Kön">Kön</label><br>
    <input type="radio" id="kvinna" name="kon" value="kvinna" v-model="orderForm.gender" checked="checked">
    <label for="kvinna">Kvinna</label><br>

    <input type="radio" id="man" name="kon" value="man" v-model="orderForm.gender">
    <label for="man">Man</label><br>

    <input type="radio" id="annat" name="kon" value="annat" v-model="orderForm.gender">
    <label for="annat">Vill ej svara</label>
</p></section>

<button type="submit" @click="submitOrder">
    <img src="/img/hungry.jpeg" alt="Hungrig smiley">
    Skicka beställning
</button>

</main>
<footer>
<hr>
    &copy; 2025 Nom Nom Burgers.
</footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, url, kCal, ingredients, gluten, lactose, meat, price) {
  this.name = name;
  this.url = url;
  this.kCal = kCal;
  this.ingredients = ingredients;
  this.gluten = gluten;
  this.lactose = lactose;
  this.meat = meat;
  this.price = price;
}


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      location: { x: 0, y: 0 },
      burgers: menu,
      orderedBurgers: {},
      orderForm: {
              name: '',
              email: '',
              street: '',
              number: '',
              payment: 'Swish',
              gender: 'kvinna'
            }
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
    },
    submitOrder() {
        console.log("Order Form Values:", this.orderForm);
        console.log("Current ordered burgers:", this.orderedBurgers);
      },
     addToOrder(event) {
         this.orderedBurgers[event.name] = event.amount;
         console.log("Current ordered burgers:", this.orderedBurgers);
       }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-size: 12pt;
    font-family: 'Agbalumo', sans-serif;
}

h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}
 footer, nav ul {
    max-width: 40rem;
    margin: 0 auto 0 auto;
}


/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

header {
    background-size: cover;
    overflow: hidden;
    height: 300px;
    margin: 50px 20px 50px 20px;
}

header img {
    height: auto;
    width: 100%;
    opacity: 0.5;
    object-fit: cover;

}


header h1 {
    margin: 0 auto;
    text-align: center;
    position: absolute;
    margin-top: -250px;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.allergier {
   margin-left: 0px;
   font-weight: bold;
   border: none;
}


#burgare-info {
    background-color: mistyrose;
    color: black;
}

button:hover {
   color: green;
}

section {
    margin: 50px 20px 50px 20px;
    border: 5px solid pink;

}

button {
    margin-left: 20px;
    margin-top: -35px;

}

div {
    padding: 20px 20px ;
    margin: 10px 80px 10px 80px;
}

.wrapper {
    margin-left: -10px;
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 450px 450px 450px;
}


div img{
    height: 250px;
    width: 220px;
}

#rubrikbild {
     height: 300px;
     width: 2000px;
}

#kartobjektet {
  width: auto;
  height: 600px;
  overflow: scroll;
  
 
}

#map {
  width: 1920px;
  height: 1072px;
  background: url("/img/polacks.jpg") ; }

</style>