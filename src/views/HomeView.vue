<template>
  <div>
    <div>
      <body>
        <head>
          <title>Burger Online</title>
          <meta charset="utf-8" />
        </head>
        <header class="header">
          <h1>Welcome to Burger Online</h1>
        </header>
        <nav></nav>
        <!-- Handels the burger part of the website-->
        <main>
          <section class="burgers">
            <h2>Select burger</h2>
            <p>This is where you execute burger selection</p>
            <section id="wrapper">
              <Burger
                v-for="burger in burgers"
                v-bind:key="burger.name"
                v-bind:burger="burger"
                v-on:orderedBurgers="addToOrder()"
              />
            </section>
          </section>
          <!--Handels the Orders part of the website-->
          <!--v-on:orderedBurger="addToOrder($event)"-->
          <section class="order">
            <form>
            <h2>Customer information</h2>
            <p>This is where you provide necessary information</p>

            <h3>Delivery information</h3>
            <div class="delivery">
              <p>
                <label for="full-name">Full name</label><br />
                <input
                  type="text"
                  id="full-name"
                  v-model="information.fulln"
                  required="required"
                  placeholder="First and last name"
                />
              </p>
              <p>
                <label for="email">E-mail</label><br />
                <input
                  type="email"
                  id="email"
                  v-model="information.email"
                  required="required"
                  placeholder="E-mail address"
                />
              </p>
            </div>

            <div class="payment">
              <p>
                <label for="payment-mehtod">Payment</label><br />
                <select v-model="information.payment">
                  <option>Pay-pal</option>
                  <option>Bank transaction</option>
                  <option>Card payment</option>
                  <option selected="selected">Swish</option>
                </select>
              </p>
            </div>
            <div class="gender">
                <label for="gender">Gender</label><br />
                <input type="radio" id="male" name="gender" value="Male" />
                <label for="male">Male</label><br />
                <input type="radio" id="female" name="gender" value="Female" />
                <label for="female">Female</label><br />
                <input
                  type="radio"
                  id="non-binary"
                  name="gender"
                  value="Non-binary"
                />
                <label for="non-binary">Non-binary</label><br />
                <input
                  type="radio"
                  id="unspecified"
                  name="gender"
                  value="Unspecified"
                  checked="checked"
                />
                <label selected="selected" for="unspecified">Unspecified</label
                ><br />
            </div>
          </form>
          </section>
          <div class="orderButton">
            <input
              v-on:click="console.log( orderdBurgers)"
              type="button"
              value="Place Order"
              id="button"
            />
          </div>
        </main>

        <footer>
          <hr />
          <span>&copy; BurgersOnline Inc, 2023</span>
        </footer>
      </body>
    </div>
    <div class="mapContainer">
      <div id="map" v-on:click="setLocation">
        <div
          v-bind:style="{
            left: this.location.x + 'px',
            top: this.location.y + 'px',
          }"
          >
          T
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import Burger from "../components/OneBurger.vue";
import io from "socket.io-client";
import menu from "../assets/menu.json";

const socket = io();

/*
function menuItem(name, kCal, gluten, plastic, lactose, url) {
  this.name = name;
  this.kCal = kCal;
  this.gluten = gluten;
  this.plastic = plastic;
  this.lactose = lactose;
  this.imgURL = url;
}

const burgers = [
  new menuItem(
    "Pink-Burger",
    0,
    false,
    true,
    false,
    "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ6yVNEQh5HTdapwb58pFFFKlC5zkc3F6VL_w&s"
  ),
  new menuItem(
    "Classic-burger",
    450,
    true,
    false,
    true,
    "https://previews.123rf.com/images/nevodka/nevodka1203/nevodka120300181/12772736-hamburger.jpg"
  ),
  new menuItem(
    "Cheese-burger",
    350,
    false,
    false,
    false,
    "https://cdn.pixabay.com/photo/2023/04/25/23/13/hamburger-7951202_1280.jpg"
  ),
];
*/
export default {
  name: "HomeView",
  components: {
    Burger,
    
  },
  data: function () {
    return {
      burgers: menu,
      information:{
    fulln: "",
    email: "",
    payment: "",
    gender: "",
  },
  orderdBurgers: {},

  location: { x: 0,
            y: 0
          },
    };
  },
  

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    submitOrder: function (event) {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y,
          fulln:  this.information.fulln,
          email:  this.information.email,
          payment:this.information.payment,
          gender: this.information.gender,

        },
        orderItems: this.orderdBurgers,
      });
    },
    
    addToOrder: function(event){
      this.orderdBurgers[event.name] = event.amount;
    },
  },
};
</script>

<style>
body {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-style: italic;
  font-size: 12pt;
}

a,
strong,
em {
  color: #0099ff;
}
/* Changes font for h1 header */
h1 {
  position: relative;
  padding-top: 50px;
  padding-bottom: 75px;
  font-family: "Bungee Spice", sans-serif;
  font-size: 40pt;
  text-align: center;
}
h2,
h3 {
  font-size: 30pt;
  text-align: center;
}

/* Boldens key allergic tags */
#allergy {
  text-transform: uppercase;
  font-weight: bold;
}
/* Add whtie text with black backround to burgers section also aligns things centered to where they are position, eg the text below the burgers*/
.burgers {
  text-align: center;
  border: 5px rgb(187, 159, 159) dashed;
  color: #ffffff;
  background-color: #000000;
  margin: 5px;
}

#wrapper {
  grid-template-columns: 300px 300px 300px;
  margin: 5px;
  display: grid;
}

.Pink-burger {
  grid-column: 1;
}
.Classic-burger {
  margin: 5px;
  padding: 5px;
  grid-column: 2;
}
.Cheese-burger {
  grid-column: 3;
}
.burgerName {
  text-align: center;
  margin: 5px;
}
.burgerImage {
  margin: 5px;
  padding: 5px;
}

.order {
  text-align: center;
  border: 5px #000000 dashed;
  padding: 5px;
  font-size: 20pt;
  margin: 5px;
}
.delivery {
  display: block;
  text-align: center;
  padding: 5px;
}
.payment {
  display: block;
  text-align: center;
  padding: 5px;
}
.gender {
  display: block;
  text-align: center;
  padding: 5px;
}
.header {
  position: relative;
  margin: 5px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 1;
}
.header::before {
  content: "";
  background-image: url(../../public/barimage.jpg);
  background-size: cover;
  position: absolute;
  top: 0px;
  bottom: 0px;
  left: 0px;
  right: 0px;
  opacity: 0.75;
}

.orderButton {
  background-color: #000000;
  text-align: center;
  padding: 5px;
  margin: 5px;
}

#button:hover {
  color: blue;
  cursor: pointer;
}

p.info,
#login p {
  width: 80%;
  margin: 1vw auto;
}
#map {
  cursor: crosshair;
  height: 1078px;
  width: 1920px;
  position: relative;
  background: url("C:/Users/Johan/1md031-lab-2023/1md031-lab-2023/public/img/polacks.jpg");
}
#map div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}
.mapContainer{
  margin: 5px;
  overflow: scroll;
  height: 500px;
  width:auto;
}
</style>
