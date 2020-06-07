<template>
  <div>
     <h2 class="text-2xl font-bold leading-7 text-red-500 sm:text-3xl sm:leading-9 sm:truncate mb-8">
      Dota 2 Counter pick
    </h2>
        <div class="my-1 flex text-white">
            <img class="inline-block h-12 w-auto rounded-lg" :src="'http://cdn.dota2.com' + hero.img" alt="" />
            <span class="ml-2 text-white leading-6 font-semibold mr-2">{{ hero.localized_name }}</span>
    </div>
  </div>
</template>
<script>
    const heroes = require('@/assets/constants/heroes.json')

    export default {
        name: 'SearchHero',
        props: {
            //msg: String
        },
        data: function () {
            return {
                heroesId : this.$route.params.id,
                heroes : heroes,
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
        }
    }
</script>
