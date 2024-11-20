<template>
    <div class="oneburger">
        <p>{{ burger.productName }}, {{ burger.kcal }} kCal</p> 
        <img v-bind:src="burger.imageUrl" />
        <p v-if="burger.sideEffects"><i>Probable side-effects:</i></p>
          <ul>
              <li v-for=" (effect, idx) in burger.sideEffects" v-bind:key="idx">{{ effect }}</li>
            </ul>
            <section class="allergies">
                <p v-if="burger.isGluten || burger.isLactose">Contains:
                  <span v-if="burger.isGluten">gluten</span>
                  <span v-if="burger.isLactose && burger.isGluten"> & </span>
                  <span v-if="burger.isLactose">lactose</span>

                </p>
                
            </section>
    </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>



.oneburger {
    background-color: #444;
    color: #fff;
    border-radius: 20px;
    padding: 20px;
    text-align: left;
    align-items: center;
    grid-template-rows: 1fr auto; 
   
}

.oneburger img {
    max-width: 90%;
    max-height: auto;
    border-radius: 10px;
    align-items: center;
    display: block; /* Gör att bilden kan centreras */
    margin-left: auto; /* Centrerar bilden horisontellt */
    margin-right: auto; /* Centrerar bilden horisontellt */
}



.allergies {
    font-weight: bold;
    text-transform: uppercase;
}


    


</style>