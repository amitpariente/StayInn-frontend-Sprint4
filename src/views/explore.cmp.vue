<template>
  <section v-if="staysToDisplay" class="explore-app main-layout-expore">
    <div class="explore-details">
      <stay-filter />
      <span>{{ staysLength }} stays</span>

    </div>
    <stay-list :stays="staysToDisplay" />
  </section>
  <app-footer />
</template>
 <script>
 import stayFilter from '../components/stay-filter.cmp.vue';
 import stayList from '../components/stay-list.cmp.vue';
 import appFooter from '../components/app-footer.cmp.vue';
 
 export default {
   name: 'explore',
   components: {
     stayFilter,
     stayList,
     appFooter
   },
   data() {
     return {
       staysLength: null
     };
   },
   created() {
     this.$store.commit({ type: 'makeSearchSmall' })
   },
   methods: {
     setSearchSmall() {
       this.$store.commit({ type: 'toggleSearchBig' })
     },
   },
   computed: {
     staysToDisplay() {
       let stays = this.$store.getters.stays
       this.staysLength = stays?.length
       return stays
     },
     getIsSearchBig() {
       return this.$store.getters.getSearch
     }
   },
 
   unmounted() { },
 };
 </script>
