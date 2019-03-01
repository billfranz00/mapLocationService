<template>
    <div class="here-map">
        <!-- Map placeholder goes here... -->
        <div ref="map" class="map"></div>
    </div>
</template>

<script>
    export default {
        name: "HereMap",
        props: {
            hereApp: {
                type: Object,
                required: false
            }
        },
        data() {
            return {
                platform: {},
                map: {}
            };
        },
        created() {
            // Initialize the HERE platform...
            this.platform = new H.service.Platform({
                "app_id": this.hereApp.id,
                "app_code": this.hereApp.code
            });
            this.geocoder = this.platform.getGeocodingService();
        },
        mounted() {
            // Replace the map placeholder with a map...
            this.map = new H.Map( // Replaces the placeolder HTML element
                this.$refs.map,
                this.platform.createDefaultLayers().normal.map,
                {
                    zoom: 10,
                    center: { lat: 37.7397, lng: -121.4252 }
                }
            );

            // this.dropMarker(37.7397, -121.4252);
        },
        methods: {
            // Interactive methods go here...
            dropMarker(latitude, longitude, searchName) {
                let marker = new H.map.Marker({ lat: latitude, lng: longitude });
                // console.log(marker);
                this.$emit('marker', { lat: latitude, lng: longitude, name: searchName });
                this.map.addObject(marker);
            },
            // drawLinesBetweenMarkers(start, middle, finish) {
            drawLinesBetweenMarkers(markers) {
                let linestring = new H.geo.LineString();
                // linestring.pushPoint(start);
                // linestring.pushPoint(middle);
                // linestring.pushPoint(finish);
                markers.forEach(marker => {
                    linestring.pushPoint(marker);
                });
                this.map.addObject(new H.map.Polyline(
                    linestring, { style: { strokeColor: "green", lineWidth: 5 }}
                ));
            },
            geocode(query) {
                return new Promise((resolve, reject) => {
                    this.geocoder.geocode({ searchText: query }, result => {
                        if(result.Response.View[0].Result.length > 0) {
                            resolve(result.Response.View[0].Result[0].Location.DisplayPosition);
                        } else {
                            reject({ message: "no results found" });
                        }
                    }, error => {
                        reject(error);
                    });
                });
            }
        }
    }
</script>

<style scoped>
    .map {
        width: 800px;
        height: 600px;
    }

    .here-map {
        float: left;
    }
</style>
