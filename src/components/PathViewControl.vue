<template lang="html">
  <div>
    <b>Display options:</b><br/>
    <div class="settings toolbox">
        <b>Spread:</b>
        <input type="checkbox" v-model="spread" id="fan-power"> &nbsp;
        <b>Paths per disc:</b>
        <select id="paths-shown" v-model="pathsShown">
          <option value="all">All</option>
          <option value="one" >One</option>
          <option value="none">None</option>
        </select> &nbsp; &nbsp;
        <b>Labels:</b>
        <input type="checkbox" id="lie-distance" v-model="lieDistance" > &nbsp; <b>Circles (10m/15m):</b>
        <input type="checkbox" id="lie-circle" v-model="lieCircle">
        <!-- &nbsp; <b>Debug:</b> <input type="checkbox" id="debug-data"> -->
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            pathsShown: "all",
            spread: true,
            lieDistance: true,
            lieCircle: true
        }
    }
}
</script>

<style lang="css">
</style>
