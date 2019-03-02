<template>
    <div id="app">
        <!-- HERE map component here... -->
        <HereMap ref="map" :hereApp="hereApp" @marker="markers.push($event)" />
        <locations :markers="markers" @addLocation="injectLocation($event)" />
    </div>
</template>

<script>
    import HereMap from "./components/HereMap.vue";
    import Locations from "./components/Locations.vue";

    export default {
        data() {
            return {
                hereApp: {
                    id: 'AvhohOpwGpQluuTLCPyq',
                    code: '50l8Xwj8r8Q-YWc-2LIW9A'
                },
                markers: []
            }
        },
        name: "app",
        components: {
            HereMap,
            Locations
        },
        async mounted() { // async|await used for asynchronous functions
            // Logic here...
            let map = this.$refs.map,
                tracy = await map.geocode("Tracy, CA"),
                manteca = await map.geocode("Manteca, CA"),
                modesto = await map.geocode("Modesto, CA");
            map.dropMarker(tracy.Latitude, tracy.Longitude, "Tracy, CA");
            map.dropMarker(manteca.Latitude, manteca.Longitude, "Manteca, CA");
            map.dropMarker(modesto.Latitude, modesto.Longitude, "Modesto, CA");
            // map.dropMarker(37.7397, -121.4252);
            // map.dropMarker(38, -121.4252);
            // map.drawLinesBetweenMarkers({ lat: 37.7397, lng: -121.4252 }, { lat: 38.7397, lng: -121.4252 });
            // map.drawLinesBetweenMarkers({lat: tracy.Latitude, lng: tracy.Longitude}, {lat: manteca.Latitude, lng: manteca.Longitude}, {lat: modesto.Latitude, lng: modesto.Longitude}); // connects all the dots to any markers that exist on the map.
            map.drawLinesBetweenMarkers(this.markers);
            map.drawRoute(manteca, modesto);
        },
        methods: {
            async injectLocation(location) {
                let map = this.$refs.map,
                    coordinates = await map.geocode(location);
                map.dropMarker(coordinates.Latitude, coordinates.Longitude, location);
            }
        }
    }
</script>

<style></style>
