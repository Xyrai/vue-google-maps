<template>
  <div class="flex flex-col relative">
    <button class="p-4 bg-yellow-500 text-black shadow-md z-10 absolute bottom-1 left-1" @click="changeType()">
      Change type
    </button>
    <Map class="flex-grow" :listings="listings" />
  </div>
</template>

<script setup lang="ts">
const listings = ref([{
  price: 123,
  coordinates: { lat: 52.20936, lng: 5.970745 }
},
{
  price: 322,
  coordinates: { lat: 52.20936, lng: 5.970745 }
},
{
  price: 1,
  coordinates: { lat: 51.209, lng: 5.97 }
}])

const changeType = () => {
  listings.value = [{
    price: 123,
    coordinates: { lat: 51.60936, lng: 5.970745 }
  },
  {
    price: 322,
    coordinates: { lat: 51.60936, lng: 5.970745 }
  },
  {
    price: 1,
    coordinates: { lat: 53.209, lng: 5.97 }
  }]
}
</script>
