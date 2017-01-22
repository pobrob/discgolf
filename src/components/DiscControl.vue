<template lang="html">
  <div>
    <b>Throw:</b><br/>
    <div class="settings toolbox">
        <b>Type:</b>
        <select id="throw-type" v-model="throwType">
          <option value="rhbh" >RHBH/LHFH</option>
          <option value="rhfh">RHFH/LHBH</option>
        </select> &nbsp;
        <b>Power:</b>
        <input type="range" min="0" max="48" v-model="power" step="1" id="power">
        (<b><span id="pwrval"></span>%</b> of nominal airspeed required by disc)
    </div>
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
