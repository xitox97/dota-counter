<template>
  <div class="vld-parent">
     
    <h2 class="text-2xl font-bold leading-7 text-red-500 sm:text-3xl sm:leading-9 sm:truncate mb-8">
      Dota 2 Counter pick
    </h2>
    <div class="my-1 flex text-white justify-center mb-6">
      <div>
        <img class="inline-block h-14 w-auto rounded-lg" :src="'http://cdn.dota2.com' + hero.img" alt="" />
        <p class="ml-2 text-white leading-6 font-semibold mt-2">{{ hero.localized_name }}</p>
      </div>
    </div>
    <div class="flex flex-col items-start">
      <div>
        <h2 class="text-sm font-semibold leading-7 text-green-600 sm:text-3xl sm:leading-9 sm:truncate mb-8">
          Good against
        </h2>
      </div>
      <loading :active.sync="isLoading" 
        :is-full-page="fullPage" color="#dc3545"></loading>
      <div class="my-1 flex text-white space-y-2 flex-wrap">
        <div v-for="(hero, index) in results" :key="hero.hero_id">
          <span v-if="index < 15">
            <img class="inline-block h-12 w-auto rounded-lg" :src="'http://cdn.dota2.com' + heroes[hero.hero_id].img" alt="" />
            <div>
              <span class="ml-2 text-white leading-6 font-semibold mr-2">{{ heroes[hero.hero_id].localized_name }}</span>
              <span class="ml-2 text-white leading-6 font-semibold mr-2">{{ hero.percents }}</span>
            </div>
          </span>
        </div>
      </div>
    </div>

    <button class="bg-transparent hover:bg-red-500 text-red-400 font-semibold hover:text-white py-2 px-4 border border-red-500 hover:border-transparent rounded">
      <router-link to="/">Search Again</router-link>
    </button>
    
  </div>
</template>
<script>
  import axios from 'axios';
  import Loading from 'vue-loading-overlay';
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
        results: null,
        isLoading: true,
        fullPage: true
      };
    },
    components: {
          Loading
      },
    computed: {
      hero() {
        var items = this.heroes;
        var result = Object.keys(items)
          .map(key => items[key])
          .filter(item => item.id == this.heroesId);
        return result[0];
      }
    },
    created() {
      this.isLoading = true;
      axios.get(`https://api.opendota.com/api/heroes/` + this.heroesId + `/matchups`)
        .then(response => {
          var matchup = response.data;
          var result = {};

          Object.keys(matchup).forEach(key => {
            const item = matchup[key];
            if (item.games_played > 99) {
              var percents = item.wins / matchup[key].games_played * 100;
              if (percents > 50) {
                item["percents"] = percents.toFixed(2);
                result[key] = item;
              }
            }
          })
          this.results = result;
          // var finalResult = [];
          // Object.keys(this.results).forEach(key => {
          //   finalResult.push(Object.keys(this.heroes)
          //   .map(key => this.heroes[key])
          //   .filter(item => item.id == this.results[key]['hero_id']));
          // });

          console.log(this.heroes[3]);
          this.isLoading = false

          //console.log(this.heroes[1]);
        })
        .catch(e => {
          //this.errors.push(e)
          console.log(e)
        this.isLoading = false

        })

    }
  }
</script>