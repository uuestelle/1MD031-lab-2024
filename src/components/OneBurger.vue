<template>
  <div class="menuitems">

    <div style="display: inline-block" class="burger"> 
    <h3>{{ burger.name }}</h3>
    <img :src="burger.image" :title="burger.name" style="width: 200px">
    <ul>
      <li v-for="ingredient in burger.ingredients" :key="ingredient">
      {{ingredient}}
      </li> 
    </ul>

    <dl>
      <dt>Allergens</dt>
      <dd v-for="allergen in burger.allergens" :key="allergen">
        <span class="allergen">{{allergen}}</span>
      </dd>
    </dl>

    <div class ="amountOrdered" >
      <button type="button" 
            id="decreaseAmount"
            @click="removeBurger">
            -
            </button>
      <p> {{amountOrdered}}  </p>
      <button type="button" 
            id="increaseAmount"
            @click="addBurger">
            +
            </button>
    </div>

  </div>
  </div>
                
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  methods: {
    addBurger: function() {
      this.amountOrdered++;
      this.$emit('orderedBurger', {burger: this.burger.name, amount: this.amountOrdered});
    },
    removeBurger: function() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', {burger: this.burger.name, amount: this.amountOrdered});
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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

  .amountOrdered {
      
      margin-top: 30px;
      display: flex;
      align-items: center;
      
  }

  .amountOrdered button {
      font-weight: bold;
      margin: 10px;
  }
</style>