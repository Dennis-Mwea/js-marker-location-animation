<template>
	<div class="App" />
</template>

<script>
import MarkerClusterer from '@google/markerclusterer'
import gmapsInit from './utils/gmaps'
const locations = [
	{
		position: {
			lat: 48.16091,
			lng: 16.38333
		}
	}
]
export default {
	name: `App`,
	async mounted() {
		let vm = this

		try {
			const google = await gmapsInit()

			let map = new google.maps.Map(this.$el, {
				zoom: 15,
				center: new google.maps.LatLng(-1.20832, 36.878746)
			})

			let marker = new google.maps.Marker({
				map: map
			})

			map.addListener('click', function(e) {
				vm.animatedMove(marker, 0.5, marker.position, e.latLng)
			})

			marker.setPosition(map.getCenter())
		} catch (error) {
			// eslint-disable-next-line no-console
			console.error(error)
		}
	},

	methods: {
		animatedMove: function(marker, t, current, moveTo) {
			var lat = current.lat()
			var lng = current.lng()

			var deltalat = (moveTo.lat() - current.lat()) / 100
			var deltalng = (moveTo.lng() - current.lng()) / 100

			var delay = 10 * t
			for (var i = 0; i < 100; i++) {
				;(function(ind) {
					setTimeout(function() {
						var lat = marker.position.lat()
						var lng = marker.position.lng()
						lat += deltalat
						lng += deltalng
						var latlng = new google.maps.LatLng(lat, lng)
						marker.setPosition(latlng)
					}, delay * ind)
				})(i)
			}
		}
	}
}
</script>

<style>
html,
body {
	margin: 0;
	padding: 0;
}
.App {
	width: 100vw;
	height: 100vh;
}
</style>
