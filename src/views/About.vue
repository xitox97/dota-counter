<template>
  <div>
    <h2 class="text-2xl font-bold leading-7 text-red-500 sm:text-3xl sm:leading-9 sm:truncate mb-8">
      Dota 2 Counter pick
    </h2>
    <div class="my-1 flex text-white justify-center mb-6">
      <div>
        <img class="inline-block h-14 w-auto rounded-lg" :src="'http://cdn.dota2.com' + hero.img" alt="" />
        <p class="ml-2 text-white leading-6 font-semibold mt-2">{{ hero.localized_name }}</p>
      </div>

    </div>

    <button class="bg-transparent hover:bg-red-500 text-red-400 font-semibold hover:text-white py-2 px-4 border border-red-500 hover:border-transparent rounded">
      <router-link to="/">Search Again</router-link>
    </button>
  </div>
</template>
<script>
  import axios from 'axios';
  const heroes = require('@/assets/constants/heroes.json')

  export default {
    name: 'SearchHero',
    props: {
      //msg: String
    },
    data: function () {
      return {
        heroesId: this.$route.params.id,
        heroes: heroes,
      };
    },
    computed: {
      hero() {
        var items = this.heroes;
        var result = Object.keys(items)
          .map(key => items[key]) // turn an array of keys into array of items.
          .filter(item => item.id == this.heroesId) // filter that array,
        return result[0];
      }
    },
    created() {
      axios.get(`https://api.opendota.com/api/heroes/` + this.heroesId + `/matchups`)
        .then(response => {
          // JSON responses are automatically parsed.
          console.log('masuk');
          var matchup = response.data;
           var result = {}
          // var result = Object.keys(matchup)
          //           .map(key => matchup[key].wins / matchup[key].games_played * 100) // turn an array of keys into array of items.
          Object.keys(matchup).forEach(key => {
            const item = matchup[key]
            if(item.games_played > 99){
            var percents = item.wins / matchup[key].games_played * 100;
            item["percents"] = percents.toFixed(2);
            result[key] = item
            }
          })
          console.log(result)
        })
        .catch(e => {
          //this.errors.push(e)
          console.log(e)
        })


    }
  }
</script>