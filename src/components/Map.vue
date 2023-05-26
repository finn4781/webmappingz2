<script>

import markers from '../assets/data/random_points.json'
import mapboxgl from 'mapbox-gl'
import FilterModalVue from '../subcomponents/FilterModal.vue'


export default {
  components: {FilterModalVue},
  data() {
    return {
      map: {},
      points: {},
      popup: {}, 

    }
  }, 
  methods: {

    applyFilter(data) {
      
      let color = data.color;
      let priceRange = data.priceRange; 
      let min, max, customFilter;

      
      if(priceRange == 'a') {min=50000; max=70000};
      if(priceRange == 'b') {min=70000; max=90000};
      if(priceRange == 'c') {min=90000; max=150000};

      if(color == '' && priceRange == '') {
        customFilter = ['!=', 'color', 'violet'] // violet is just a dummy example 

      } else if(color == '') {
        // price filter 
        customFilter = ["all", ['>=', 'price', min], ['<=', 'price', max] ]
      } else if(priceRange == '') {
        // color filter
        customFilter = ['==', 'color', `${color}`]
      } else {
        customFilter = ["all", ['==', 'color', `${color}`], ['>=', 'price', min], ['<=', 'price', max] ]
      };

      this.map.removeLayer('markers-layer');

      this.map.addLayer({
        'id': 'markers-layer',
        'type': 'circle',
        'source': 'points',
        'paint': {
          'circle-color': ['get','color'],
          'circle-radius': 8,
          'circle-stroke-color': 'black',
          // 'circle-stroke-color': ['get','color'],
          'circle-stroke-width': 2,
          'circle-stroke-opacity': 0.5,
          // 'circle-translate': [20,10],
          // 'circle-blur': 0,
          // 'circle-opacity'
        },
        // 'filter': ["all", ['==', 'color', `${this.color}`], ['>=', 'price', min], ['<=', 'price', max] ],
        'filter': customFilter,
      })

    }, 

    adjustMapHeight() {
      this.points = markers;
      let screenHeight = document.documentElement.clientHeight,
          navbarHeightpx = getComputedStyle(document.getElementById('navbarSection')).height,
          navbarHeight = parseInt(navbarHeightpx.split('px')[0]),
          mapHeight = screenHeight - navbarHeight; 
      document.getElementById('map').style.height = `${mapHeight}px`;

    }, 
    initiateMap() {
      mapboxgl.accessToken = 'YOUR-MAPBOX-TOKEN';
      this.map = new mapboxgl.Map({
        container: 'map', 
        // style: 'mapbox://styles/mapbox/streets-v12',
        style: 'mapbox://styles/mapbox/satellite-v9',
        center: [-0.1148, 51.500], 
        zoom: 11, 
      });
      this.map.addControl(new mapboxgl.NavigationControl(), 'bottom-right');
      this.map.addControl(new mapboxgl.ScaleControl(), 'bottom-left')

      // add markers 
      this.map.on('load', () => {
        this.map.addSource('points', {
          'type': 'geojson', 
          'data': this.points
        })

        this.map.addLayer({
          'id': 'markers-layer',
          'type': 'circle',
          'source': 'points',
          'paint':{
            'circle-color': ['get','color'],
            'circle-radius': 8,
            'circle-stroke-color': 'black',
            // 'circle-stroke-color': ['get','color'],
            'circle-stroke-width': 2,
            'circle-stroke-opacity': 0.5,
            // 'circle-translate': [20,10],
            // 'circle-blur': 0,
            // 'circle-opacity'          
          }
        })

        this.map.on('click', 'markers-layer', (e) => {
          
          const title = e.features[0].properties.title;
          const price = e.features[0].properties.price;
          const description = e.features[0].properties.description;
          const image = e.features[0].properties.image;
          document.getElementById('ocContent').innerHTML = `
            <img src="images/${image}" alt="no image" class=" d-block mx-auto img-fluid">
            <div class="text-center mx-auto " >
              <h3 class="mt-2">${title}</h3>
              <h3 class="text-secondary">price: ${price} $</h3>
              <p >${description}</p>
              <a type="button" class="btn btn-success text-white rounded-3" style="background-color: green;" href="https://wa.me/1999999999?text=I'm%20interested%20in%20your%20house%20for%20sale" target="_blank">sit emmet !</a>
            </div>
          `;
          document.getElementById('offcanvasButton').click();
          
        });

        // Create a popup, but don't add it to the map yet.
        this.popup = new mapboxgl.Popup({
          closeButton: false,
          closeOnClick: false
        });

        this.map.on('mouseenter', 'markers-layer', (e) => {
          // Change the cursor style as a UI indicator.
          this.map.getCanvas().style.cursor = 'pointer';
          
          // Copy coordinates array.
          const coordinates = e.features[0].geometry.coordinates.slice();
          // const description = e.features[0].properties.description;
          const title = e.features[0].properties.title;
          const price = e.features[0].properties.price;
          const popcontent = `
            <div class="card text-bg-primary" >
              <div class="card-body">
                <h5 class="card-title">${price}</h5>
                <h6 class="card-subtitle mb-2 ">${title}</h6>
              </div>
            </div>
          `;
          this.popup.setLngLat(coordinates).setHTML(popcontent).addTo(this.map);
        });
          
        this.map.on('mouseleave', 'markers-layer', () => {
          this.map.getCanvas().style.cursor = '';
          this.popup.remove();
        });

      })

    },

  },
  mounted() {
    this.adjustMapHeight();
    this.initiateMap();
  },
}

</script>


<template>

  <div id="map-container" class="position-relative">

    <div id="map" style="width: 100%"></div>
    <button id="offcanvasButton" class="d-none" data-bs-toggle="offcanvas" data-bs-target="#offcanvasExample" aria-controls="offcanvasExample"></button>
    <button type="button" class="btn btn-primary m-2 border border-dark position-absolute top-0 start-0" data-bs-toggle="modal" data-bs-target="#filterModal">
      Filter</button>

  </div>

  <FilterModalVue @apply-filter="applyFilter($event)"/>

</template>


<style scoped>

</style>
