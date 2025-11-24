<template>
              <div class="one-burger">
                <h2>{{ burger.name }}</h2>
                <img :src="'/img/' + burger.url" :alt="burger.name" :title="burger.name">
                <dl>
                  <dt>Med smak av:</dt>
                  <ul>
                    <li v-for="(ingredient, index) in burger.ingredients" :key="index">{{ ingredient }}</li>
                  </ul>
                  <section class="allergier">
                    <dt>Allergener:</dt>
                    <ul>
                      <li v-if="burger.lactose">Laktos</li>
                      <li v-if="burger.gluten">Gluten</li>
                      <li v-if="burger.meat">{{ burger.meat }}</li>
                    </ul>
                  </section>
                  <p>Pris: {{ burger.price }} kr</p>
                  <p>Energi: {{ burger.kCal }} kcal</p>
                  <p>Antal beställda: {{ amountOrdered }}</p>
                  <button @click="decreaseAmount">-</button>
                  <button @click="increaseAmount">+</button>
                </dl>
              </div>

</template>
<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
    data() {
      return {
        amountOrdered: 0
      }
    },
    methods: {
      increaseAmount() {
        this.amountOrdered++;
        this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered });
      },
      decreaseAmount() {
        if (this.amountOrdered > 0) {
          this.amountOrdered--;
          this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered });
        }
      }
    }
  }
</script>

<style scoped>

nav ul {
    max-width: 40rem;
    margin: 0 auto 0 auto;
}


nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}



.allergier {
   margin-left: 0px;
   font-weight: bold;
   border: none;
}


div {
    padding: 20px 20px ;
    margin: 10px 80px 10px 80px;
}

</style>
