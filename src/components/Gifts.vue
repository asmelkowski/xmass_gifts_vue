<template>
  <div class="gifts">
    <template v-for="gift in gifts">
      <div class="gift" v-if="gift.taken == 0">
        <div class="gift-name">
          {{ gift.name }}
        </div>
        <button @click="take(gift.id, gift.name)">Zajęty/a</button>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: 'Gifts',
  data() {
    return {
      gifts: null
    }
  },
  mounted() {
    this.get_gifts();
  },
  methods: {
    get_gifts: function() {
      fetch('http://localhost:5000/gifts')
      .then(stream => stream.json())
      .then(data => this.gifts = data)
      .catch(error => console.log(error))
    },
    take: function(id, name) {
      if(confirm("Na pewno chcesz oznaczyć " + name + " jako zajętego/zajętą?") == true) {
        fetch('http://localhost:5000/gifts/' + id, {
          method: 'post',
          body: JSON.stringify()
        })
          .then(response => (console.log(response)))
          .then(this.get_gifts)
          .catch(error => console.log(error));
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gift {
  margin: 1rem auto;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  width: 70vw;
}

.gift:nth-child(even){
  background: rgba(172, 155, 155, 0.062);
}
@media screen and (max-width: 500px){
  .gift {
    width: 100%;
  }
}

</style>
