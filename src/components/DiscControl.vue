<template lang="html">
  <div class="container">
    <form>
      <div class="panel panel-default">

    <div class="panel-heading">Throw:</div>
    <div class="settings toolbox panel-body">
        <label>Type:</label>
        <select id="throw-type" v-model="throwType" class="form-control">
          <option value="rhbh" >RHBH/LHFH</option>
          <option value="rhfh">RHFH/LHBH</option>
        </select> &nbsp;
        <label>Power:</b>
        <input type="range" min="0" max="48" v-model="power" step="1" id="power" class="form-control">
        (<b><span id="pwrval"></span>%</b> of nominal airspeed required by disc)
    </div>
  </form>
</div>
</template>

<script>
export default {
    data() {
        return {
            power: 32,
            throwType: "rhbh"
        }
    }
}
</script>

<style lang="css">
</style>
