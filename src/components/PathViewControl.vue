<style lang="css">



</style>

<template lang="html">

<form>
  <div class="panel panel-default">
    <div class="panel-heading">Display options:</div>
    <div class="settings toolbox panel-body">
        <div class="form-group">
          <label>Spread:</label>
          <input type="checkbox" v-model="spread" id="fan-power" class="form-check-input"> &nbsp;
        </div>
        <div class="row">
          <div class="form-group">
            <label>Paths per disc:</label>
            <select id="paths-shown" v-model="pathsShown" class="form-control">
              <option value="all">All</option>
              <option value="one">One</option>
              <option value="none">None</option>
            </select> &nbsp; &nbsp;
          </div>
        </div>
        <div class="form-group">
          <label>Labels:</label>
          <input type="checkbox" id="lie-distance" v-model="lieDistance" class="form-check-input"> &nbsp; <b>Circles (10m/15m):</b>
          <input type="checkbox" id="lie-circle" v-model="lieCircle" class="form-check-input">
        </div>
    </div>
  </div>
</form>
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
