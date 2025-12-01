<template>
  
        <header>
          <div id="headerContent">
                <img src="https://res.cloudinary.com/gordonramsay/image/upload/c_fill,w_650,h_460,q_auto,f_auto/Gordon%20Ramsay%20Burger%20US/locations/Burger_Location_Tile_2_wl3hqe" 
                title="Header image" id="headerimage">
                <h1>Welcome to Fast Burgers!</h1>
            </div>
        </header>

        <main>  
            <section class="burgermenu">   
                <h2>Fast Burgers menu</h2>
                <p>Choose from our delicious selection of burgers below:</p>

                <div class="menuitems">
                  <Burger v-for="burger in burgers" 
                  :key="burger.name" 
                  :burger="burger" 
                  @orderedBurger="addToOrder($event)"/>
                
                </div>

            </section>

            <section class="contact"> 
                <h2>Delivery information</h2>
                <p>Please fill in your delivery information here</p>
                <form>
                    <p>
                        <label for ="name">Full Name</label><br>
                        <input type="text" id="fullname" v-model="customerName" required="required" placeholder="First and Last Name">
                    </p>
                    
                    <p>
                        <label for ="email">E-mail</label><br>
                        <input type="email" id="email" v-model="customerEmail" required="required" placeholder="E-mail address">
                    </p>
                    <p>
                        <label for ="Street">Address</label><br>
                        <input type="text" id="streetname" v-model="customerStreet" required="required" placeholder="Street name">
                    </p>
                    <p>
                        <label for ="housenumber">House number</label><br>
                        <input type="number" id="housenumber" v-model="customerHouseNumber" required="required" placeholder="House number">
                    </p>
                    <p>
                        <label for="payment method">Payment method</label>
                        <select id="payment method" v-model="paymentMethod">                            
                            <option selected="selected">Debit card</option>
                            <option>Credit card</option>
                            <option>Swish</option>
                            <option>Cash</option>
                            <option>Klarna</option>                        
                        </select>
                    </p>
                    <p>
                        <label for="gender">Gender</label><br>

                        <input type="radio" id="Do not wish to provide" v-model="gender" value="Do not wish to provide" checked="checked"> 
                        <label for="Do not wish to provide">Do not wish to provide</label><br>

                        <input type="radio" id="Female" v-model="gender" value="Female">
                        <label for="Female">Female</label><br>

                        <input type="radio" id="Male" v-model="gender" value="Male"> 
                        <label for="Male">Male</label><br>

                        <input type="radio" id="Non-binary" v-model="gender" value="Non-binary">
                        <label for="Non-binary">Non-binary</label><br>

                    </p>
                    
                </form>

              <div class="deliveryMap"> 
                <h2>Delivery map</h2>
                <p>Click on the map to set delivery location:</p>

                <div id="mapWrapper">
                  <div id="map" @click="addOrder">
                    <div id="target"
                      :style="{ left: location.x + 'px', top: location.y + 'px'}">
                      📍
                    </div>
                  </div>
                </div>
            </div>

            </section>
            <button type="button" 
            style="border:None;" 
            id="submitbutton"
            @click="submitOrder">

                <img src="https://www.freeiconspng.com/uploads/submit-button-png-9.png" style="width:100px">
                
            </button>
            
        </main>

        <hr>

        <footer> 
            <p>© 2025 Fast Burgers</p>
        </footer>
  
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(nm, url, kcal, gluten, lactose, ingredients, allergens) {
  this.name = nm;
  this.image = url;
  this.calories = kcal;
  this.gluten = gluten;
  this.lactose = lactose;
  this.ingredients = ingredients;
  this.allergens = allergens;
}


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      customerName: '',
      customerEmail: '',
      customerStreet: '',
      customerHouseNumber: null,
      paymentMethod: 'Debit card',
      gender: 'Do not wish to provide',
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
      }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left +10,
                    y: event.currentTarget.getBoundingClientRect().top +10};

                    const x = event.clientX - offset.x ;
                    const y = event.clientY - offset.y;

                    this.location.x = x;
                    this.location.y = y;

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x, y },
                                orderItems: this.orderedItems
                              }
                 );
    }
    ,
    submitOrder: function () {
      console.log("Order submitted:");
      console.log("Name: " + this.customerName);
      console.log("E-mail: " + this.customerEmail);
      console.log("Street: " + this.customerStreet);
      console.log("House number: " + this.customerHouseNumber);
      console.log("Payment method: " + this.paymentMethod);
      console.log("Gender: " + this.gender);
    },

    addToOrder: function (event) {
      this.orderedBurgers[event.burger] = event.amount;
      console.log(this.orderedBurgers);
    }
  }
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
  body {
      font-size: 1.2rem;
      font-family: "Times New Roman", serif;
  }

  header {
      margin: 20px;
      height: 200px;
      overflow:hidden;
  }
  #headerimage {
      opacity: 0.5;
      width: 100%;
      height: auto;
  }

  header h1 {
      width:40rem;
      margin: 0px auto;
      text-align: center;
      position: absolute;
      padding: 60px 20px 20px 20px;
      margin-top: -990px;
  }
  h1 {
      font-family: 'Agbalumo';
      font-size: 36pt;
  }

  h2 {
      padding-left: 10px;
  }


  .menuitems {
      display: grid;
      grid-gap: 20px;
      grid-template-columns: 1fr 1fr 1fr;
      color: #fff;
  }

  .burger {
      background-color: #444;
      border-radius: 5px;
      padding: 20px;
      font-size: 100%;
  }

  .allergen {
      font-weight: bold;
  }

  .burgermenu {
      background-color: white;
      color: black;
      border: 3px dashed white;
  }

  .burgermenu p {
      margin-left: 10px;
  }

  .contact {
      border: 3px dashed black;
  }

  button:hover {
      background-color: blue;
      cursor: pointer;
  }

  section {
      margin: 20px;
  }

  #submitbutton {
      margin: 20px;
  }

  .contact p {
      margin-left: 10px;
  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    position: relative;

  }

  #mapWrapper {
    height: 300px;
    overflow: scroll;
    margin: 20px;
    position: relative;
  }

  #target {
    position: absolute;
    width: 20px;
    height: 20px;
  }
</style>