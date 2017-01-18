<template>
<div id="app">
    <h1>{{ msg }}</h1>
    <disc-discSelector></disc-discSelector>
    <disc-image></disc-image>
    <disc-flightPath></disc-flightPath>
</template>

<script>
import DiscSelector from './components/DiscSelector.vue';
import DiscImage from './components/DiscImage.vue';
import FlightPath from './components/FlightPath.vue';
export default {
    name: 'app',
    components: {
        'disc-discSelector': DiscSelector,
        'disc-image': DiscImage,
        'disc-flightPath': FlightPath
    },
    data() {
        return {
            msg: 'Welcome to the Disc Golf Simulator'


        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

h1,
h2 {
    font-weight: normal;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
