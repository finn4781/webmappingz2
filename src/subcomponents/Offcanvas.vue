
<script>
export default {
  props: ['title', 'description', 'price', 'image'],
  data() {
    return {
      photo: this.image,
    }
  },
  computed: {
    getPhoto() {
      return `/src/assets/images/${this.image}`;
    }

  }
}
</script>



<template>

<!-- OFFCANVAS -->
<div class="offcanvas offcanvas-start" tabindex="-1" id="offcanvasExample" aria-labelledby="offcanvasLabel">
  <div class="offcanvas-header my-1 py-2">
    <h5 class="offcanvas-title" id="offcanvasLabel">
      {{ title }}
    </h5>
    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
  </div>
  <div class="offcanvas-body" id="ocContent">

    <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
      <div class="carousel-inner">
        <div class="carousel-item active">
          <img :src="`images/${image}`" class="d-block w-100" alt="...">
        </div>
      
      </div>
      <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Previous</span>
      </button>
      <button class="text-info carousel-control-next" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="next" style="color: #FF1493">
        <span class="text-info carousel-control-next-icon" aria-hidden="true" style="color: #FF1493"></span>
        <span class="text-info visually-hidden" style="color: #FF1493">Next</span>
      </button>
    </div>

    <div class="d-flex justify-content-center py-2">
      Price: {{ price }} $
    </div>
    <div class="" style="font-size: 0.9em">
      {{ description}}
    </div>
    <div class="d-flex justify-content-center my-3">
      <a type="button" class="btn text-white" style="background-color:green" href="https://wa.me/19999999999?text=I'm%20interested%20in%20your%20houser%20for%20sale" target="_blank">WhatsApp</a>
    </div>
  </div>
</div>

</template>


<style scoped>

</style>