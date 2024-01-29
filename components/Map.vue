<template>
  <div class="w-full h-screen">
    <button class="p-4 bg-red-800 text-white shadow-md z-10 absolute bottom-1 left-32" @click="clearMarkers()">
      Clear markers
    </button>
    <div class="w-full h-full" ref="mapElement"></div>
  </div>
</template>

<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader'
import { MarkerClusterer } from "@googlemaps/markerclusterer"

interface Listing {
  price: number
  coordinates: {
    lat: number
    lng: number
  }
}

const props = defineProps<{
  markerData: Listing[]
}>()

const mapElement = ref<HTMLElement>()
const map = ref<google.maps.Map>()
const markers = shallowRef<google.maps.Marker[]>([])
const clusterer = shallowRef<MarkerClusterer>()
const bounds = shallowRef<google.maps.LatLngBounds>()

const mapOptions = {
  center: { lat: 52.20936, lng: 5.970745 },
  zoom: 8
}

onMounted(async () => {
  const loader = new Loader({
    apiKey: "AIzaSyCTHZllCldMYoM9ByF8AcxKPWvIuFJsTx4",
    version: "weekly",
    libraries: ["maps"]
  })

  const { Map } = await loader.importLibrary("maps")

  if (mapElement.value) {
    map.value = new Map(mapElement.value, mapOptions)
    bounds.value = new google.maps.LatLngBounds()

    setMarkers(props.markerData)

    if (bounds.value) {
      map.value.fitBounds(bounds.value)
    }
    clusterer.value = new MarkerClusterer({ map: map.value, markers: markers.value })
  }
})

const clearMarkers = () => {
  if (clusterer.value) {
    clusterer.value.clearMarkers()
  }
}

const setMarkers = (listings: Listing[]) => {
  for (const listing of listings) {
    const { coordinates } = listing

    const latLng = new google.maps.LatLng(coordinates.lat, coordinates.lng)
    const marker = new google.maps.Marker({
      position: latLng,
    })

    bounds.value?.extend(latLng)

    marker.addListener("click", () => {
      console.log("You clicked me! 😎")
    })

    markers.value.push(marker)
  }
}

watch(() => props.markerData, () => {
  clearMarkers()

  if (markers.value) {
    markers.value = []
  }

  if (props.markerData && map.value) {
    setMarkers(props.markerData)

    if (clusterer.value) {
      clusterer.value.addMarkers(markers.value)
    }

    if (bounds.value) {
      map.value.fitBounds(bounds.value)
    }
  }
})
</script>