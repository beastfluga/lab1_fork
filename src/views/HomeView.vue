

<template>

<header>
  <h1>Welcome to credit card burgers</h1>
  <h3><i>- We love credit cards</i></h3>
</header>
        <nav>Menu Items</nav>
        <main>
          <div>
            <h1>Burgers</h1>
            <section id="burgersection">
             
              
              <Burger v-for="burger in burgers"
                      v-bind:burger="burger" 
                      v-bind:key="burger.name"/>
              
                      
            </section>
              <div id="map" v-on:click="addOrder">
              click here
              </div>
          </div>
            <section id="contact">
                <h3>Customer information</h3>
                <form>
                  <p>
                        <label for="recipient">Recipient</label>
                        <select id="recipient" v-model="rcp">
                            <option>Credit card (recommended)</option>
                            <option>Master card</option>
                            <option>Nature</option>
                            <option>Sheep</option>
                            <option>Venezuelan Bolívares</option>
                            <option>Metal objects and/or coins</option>
                        </select>
                     </p>
                    <p>
                        <label for="firstname">First name</label><br>
                        <input type="text" id="firstname" v-model="fn" required="required" placeholder="First name">
                    </p>
                    <p>
                        <label for="lastname">Last name</label><br>
                        <input type="text" id="lastname" v-model="ln" placeholder="Last name">
                    </p>
                    <p>
                        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                    </p>
                    <p v-if="rcp == 'Credit card (recommended)' || rcp == 'Master card'">
                        <label for="Account number">Account number</label><br>
                        <input type="text" id="Account number" v-model="an" placeholder="ex: 1100 8765 099">
                    </p>
                    <p v-if="rcp == 'Credit card (recommended)' || rcp == 'Master card'">
                    <label for="Clearing number">Clearing number</label><br>
                        <input type="text" id="Clearing number" v-model="cn" placeholder="ex: 110085">
                    </p>
                    <p>
                        <label for="street">Street name</label><br>
                        <input type="text" id="street" v-model="strt" required="required" placeholder="street name">
                    </p>
                    <p>
                        <label for="street number">Street number</label><br>
                        <input type="number" id="street number" v-model="strnum" required="required" placeholder="street number">
                    </p>
                    <p>
                    Pick a gender <br>
                    <label>
                       <input type="radio" v-model="gender" value="Man" checked="checked">
                        Man
                    </label></br>
                    <label>
                        <input type="radio" v-model="gender" value="Woman">
                         Woman
                    </label></br>
                    <label>
                        <input type="radio" v-model="gender" value="Other">
                         other
                    </label></br>
                    <label>
                        <input type="radio" v-model="gender" value="None of your business">
                         None of your business
                    </label></br>
                    </p>
                    Hello, {{this.fn}}!
                   
                </form>
            </section>
                <button type="submit" class="button" v-on:click="getInfo(key)">Klart">
                    <img src="https://www.whitehouse.gov/wp-content/uploads/2021/01/33_harry_s_truman.jpg"
                        style="width: 70px; height: 70px; vertical-align: middle;">
                    Send
                </button>
            </main>
            <footer>
                End notes
            </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");
//object constructor function
function MenuItem(productName, imageUrl, kcal, isGluten, isLactose, sideEffects) {
  this.productName = productName;
  this.imageUrl = imageUrl;
  this.kcal = kcal;
  this.isGluten = isGluten;
  this.isLactose = isLactose;
  this.sideEffects = sideEffects;
}
const burgersList = [
  new MenuItem("Burger1", "https://i.pinimg.com/736x/0f/07/8c/0f078c8138ac3d583b6b1cfe3d325cdc.jpg", 300, false, false),
  new MenuItem("Burger2", "https://static1.businessinsider.com/image/51bb7442ecad04b71d00002b/a-disgusting-9-patty-burger-from-a-spoof-ad-is-now-banned-from-wendys.jpg", 400, true, true),
  new MenuItem("Burger3", "https://www.thescottishsun.co.uk/wp-content/uploads/sites/2/2017/06/nintchdbpict000328367792.jpg?w=1980", 7000, true, false),
];

const newburgersList = [
  ...menu
]

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: newburgersList,
      rcp: 'Credit card (recommended)',
      ln: '',
      fn: '',
      an: '',
      cn: '',
      strt: '',
      strnum: '',
      gender: '',



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
    getInfo: function() {
      console.log("method of payment:", this.rcp, "\n",
      "Last name:", this.ln, "\n", 
      "First name:", this.fn, "\n",
      "Account number:", this.an, "\n", 
      "Clearing number:", this.cn, "\n", 
      "Strret", this.strt, "\n",
      "Street number:", this.strnum, "\n",
      "Gendder:", this.gender)
    }
  }
}

</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-size: 20pt;
}



h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}
section > h3 > * {
    color:red;
}


main, header, footer, nav ul {
    /*max-width: 40rem; jag kommenterade bort pga burgarna hamna på nya rader */
    margin: 0 auto 0 auto;
}


main {
    background-color: bisque;
}
    
  
    


 nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} 

header {
    background-image: url("https://s3.amazonaws.com/piggy-blog/wp-content/uploads/2020/04/14211808/image-1.jpg");
    background-size: contain;
    overflow: hidden;
    width: 100%;
    height: auto;
    opacity: 0.5;
}

header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
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

    

main > section{
    margin-top: 60px;
    border: 2px double white;
}
button{
    margin: 50px;
}
.button:hover{
    background-color: blueviolet;
    cursor: pointer;
}

section > div{
    margin: 20px;
    padding: 20px;
}
#burgersection {
    background-color: #000000;
    color: white;
    padding: 50px;
    display: grid;
    grid-gap: 100px;
    grid-template-columns: repeat(5, 300px);
    width: 100%; /* Fyll hela skärmen */
    
}
#contact {
  font-size: 40px;
  
 
}
input[type="text"],
input[type="email"],
input[type="number"]
 {
  height: 40px;
  width: 200px;
}
select  {
  height: 40px;
  width: 200px;
  margin-left: 20px;
}
form {
  margin-left: 30px;
}
input::placeholder {
  font-size: 20px;
}

/*
#burgerdiv {
    display: grid;
    grid-gap: 20px;
    
    width: 100%; 
}

*/




</style>
