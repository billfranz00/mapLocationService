<template>
    <div class="locations">
        <h1>Your Locations</h1>
        <input type="text" name="location" v-model="location" @keyup.enter="addLocation">
        <button @click="addLocation">Add Location</button>
        <ol>
            <li>{{ NumberOne }}</li>
            <li>{{ NumberTwo }}</li>
            <!-- <li v-for="marker in markers">Location {{ marker.zb }}</li> -->
            <li v-for="marker in markers"><strong>{{ marker.name }} |</strong> Lat: {{ marker.lat }} | Lng: {{ marker.lng }}</li>
        </ol>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                NumberOne: "Numero Uno",
                NumberTwo: "",
                location: ""
            }
        },
        props: {
            markers: {
                type: Array,
                required: false
            }
        },
        name: "locations",
        mounted() {
            this.NumberTwo = "Twos are the Best";
        },
        methods: {
            addLocation() {
                let location = this.location;
                this.$emit("addLocation", location);
                this.location = "";
            }
        }
    }
</script>

<style scoped>
    .locations {
        float: left;
        margin-left: 50px;
    }
</style>
