<template lang="html">
  <div>
    <b>Display options:</b><br/>
    <div class="settings toolbox">
        <b>Spread:</b>
        <input type="checkbox" checked id="fan-power"> &nbsp;
        <b>Paths per disc:</b>
        <select id="paths-shown"><option value="all">All</option>
          <option value="one" selected>One</option>
          <option value="none">None</option>
        </select> &nbsp; &nbsp;
        <b>Labels:</b>
        <input type="checkbox" id="lie-distance" checked> &nbsp; <b>Circles (10m/15m):</b>
        <input type="checkbox" id="lie-circle" checked>
        <!-- &nbsp; <b>Debug:</b> <input type="checkbox" id="debug-data"> -->
    </div>
</div>
</template>

<script>
export default {}
</script>

<style lang="css">
</style>
