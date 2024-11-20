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
                        <label for="firstname">First</label><br>
                        <input type="text" id="firstname" name="fn" required="required" placeholder="First name">
                    </p>
                    <p>
                        <label for="lastname">Last</label><br>
                        <input type="text" id="lastname" name="ln" placeholder="Last name">
                    </p>
                    <p>
                        <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
                    </p>
                    <p>
                        <label for="Account number">Account number</label><br>
                        <input type="text" id="Account number" name="ln" placeholder="ex: 1100 8765 099">
                    </p>
                    <label for="Clearing number">Clearing number</label><br>
                        <input type="text" id="Clearing number" name="ln" placeholder="ex: 110085">
                    <p>
                        <label for="street">Street name</label><br>
                        <input type="text" id="street" name="strt" required="required" placeholder="street name">
                    </p>
                    <p>
                        <label for="street number">Street number</label><br>
                        <input type="number" id="street number" name="strnum" required="required" placeholder="street number">
                    </p>
                    <p>
                    Pick a gender <br>
                    <label>
                       <input type="radio" name="gender" value="Man" checked="checked">
                        Man
                    </label></br>
                    <label>
                        <input type="radio" name="gender" value="Woman">
                         Woman
                    </label></br>
                    <label>
                        <input type="radio" name="gender" value="Other">
                         other
                    </label></br>
                    <label>
                        <input type="radio" name="gender" value="None of your business">
                         None of your business
                    </label></br>
                    </p>
                    <p>
                        <label for="recipient">Recipient</label>
                        <select id="recipient" name="rcp">
                            <option>Credit card (recommended)</option>
                            <option>Master card</option>
                            <option>Nature</option>
                            <option>Sheep</option>
                            <option>Venezuelan Bolívares</option>
                            <option>Metal objects and/or coins</option>
                        </select>
                     </p>
                </form>
            </section>
                <button type="submit" class="button">
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

const socket = io("localhost:3000");
//object constructor function
function MenuItem(productName, imageUrl, kcal, isGluten, isLactose) {
  this.productName = productName;
  this.imageUrl = imageUrl;
  this.kcal = kcal;
  this.isGluten = isGluten;
  this.isLactose = isLactose;
}
const burgersList = [
  new MenuItem("Burger1", "https://i.pinimg.com/736x/0f/07/8c/0f078c8138ac3d583b6b1cfe3d325cdc.jpg", 300, false, false),
  new MenuItem("Burger2", "https://static1.businessinsider.com/image/51bb7442ecad04b71d00002b/a-disgusting-9-patty-burger-from-a-spoof-ad-is-now-banned-from-wendys.jpg", 400, true, true),
  new MenuItem("Burger3", "https://www.thescottishsun.co.uk/wp-content/uploads/sites/2/2017/06/nintchdbpict000328367792.jpg?w=1980", 7000, true, false),
];

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgersList

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
    font-size: 17pt;
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
    margin: 30px;
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
/*
#burgerdiv {
    display: grid;
    grid-gap: 20px;
    
    width: 100%; 
}

*/




</style>
