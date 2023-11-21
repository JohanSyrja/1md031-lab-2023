<template>
  <div>
    <div class="{{burger.name}}">
      <h2 class="burgerName">{{ burger.name }}</h2>
      <br />
      <div class="burgerImage">
        <img v-bind:src="burger.imgURL" style="width: 300px; height: 200px">
      </div>
      <button v-on:click="addOrder"></button>
      <button v-on:click="removeOrder"></button>
      <p>{{ amountOrdered }}</p>
      
      <section id="wrapper">
        <ul>
          <li>
            kCal: {{ burger.kCal }}
          </li>
          <li v-if="burger.gluten">
            Contains <span id="allergy">gluten</span>
          </li>

          <li v-if="burger.lactose">
            Contains <span id="allergy">lactose</span>
          </li>

          <li v-if="burger.plastic">
            Contains <span id="allergy">plastic</span>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: "OneBurger",
  props: {
    burger: Object,
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
},
  methods: {
    addOrder: function () {
      this.amountOrdered++;
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
      
    },
    removeOrder: function () {
      if (this.amountOrdered != 0) {
        this.amountOrdered--;
      }
      
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.burgers {
  text-align: center;
  border: 5px rgb(187, 159, 159) dashed;
  color: #ffffff;
  background-color: #000000;
  margin: 5px;
}

.wrapper {
  grid-template-columns: 300px 300px 300px;
  margin: 5px;
  display: grid;
}
.hamburger {
  color: #ffffff;
  background-color: #000000;
  border-radius: 5px;
  padding: 20px;
  font-size: 150%;
}
.a {
  grid-column: 1;
}
.b {
  grid-column: 2;
}
.c {
  grid-column: 3;
}
</style>
