<script>
import AboutModalVue from '../subcomponents/AboutModal.vue'
import TermsModalVue from '../subcomponents/TermsModal.vue'
import PrivacyModalVue from '../subcomponents/PrivacyModal.vue'
import ContactModalVue from '../subcomponents/ContactModal.vue'
// import FilterModalVue from '../subcomponents/FilterModal.vue'
import OffcanvasVue from '../subcomponents/Offcanvas.vue'


export default {
  components: {AboutModalVue, TermsModalVue, PrivacyModalVue, ContactModalVue, OffcanvasVue},
  data() {
    return {
      title: '',
      price: '',
      description: '',
      image: '',
    }
  }, 
  methods: {
    applyFilter(data) {
      // console.log('')
      this.$emit('apply-filter', data);
    }, 
  }  
}
</script>

<template>
  
<nav id="navbarSection" class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="/">Mybrandname</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <!-- Button trigger modal -->
          <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#aboutModal">
            About
          </a>

        </li>
        <li class="nav-item">
          <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#contactModal">Contact</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#privacyModal">Privacy Policy</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#termsModal">Terms of use</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<AboutModalVue/>
<TermsModalVue/>
<PrivacyModalVue/>
<ContactModalVue/>


<OffcanvasVue :title="title" :price="price" :description="description" :image="image"/>


</template>


<style scoped>

</style>