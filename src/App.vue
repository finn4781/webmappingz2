<script>
import NavbarVue from './components/Navbar.vue'
import MapVue from './components/Map.vue'

export default {
  components: {NavbarVue, MapVue},
  data() {
    return {
      color: '',
      priceRange: ''
    }
  }, 
  methods: {

  },
  mounted() {

  },
}
</script>


<template>

<NavbarVue/>
<MapVue/>

</template>



<style scoped>

</style>
