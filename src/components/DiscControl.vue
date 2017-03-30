<style lang="css">
</style>

<template lang="html">
<div>
    <form>
      <div class="panel panel-default">
          <div class="panel-heading">Throw:</div>
          <div class=" settings toolbox panel-body">
              <div class="row">
                  <div class="col-xs-12 col-sm-8 col-md-6">
                    <label>Type:</label>
                    <select id="throw-type" v-model:value="discThrowType" @change='changeThrowType' class="form-control">
                      <option value="rhbh">RHBH/LHFH</option>
                      <option value="rhfh">RHFH/LHBH</option>
                    </select> &nbsp;
                    <div class="form-group">
                      <label>Power:</label>
                      <input type="range" min="0" max="48" v-model:value="discPower" step="1" @change='changePower' class="form-control"> (
                      <b><span id="pwrval"></span>%</b> of nominal airspeed required by disc)
                    </div>
                  </div>
              </div>
          </div>
        </div>
    </form>
</div>
</template>

<script>
export default {
    data() {
        return {
            discPower: this.power,
            discThrowType: this.throwtype
        }
    },
    props: ['discs', 'power', 'throwType'],
    methods: {
        changePower(event) {
            this.$emit('changeDiscPower', this.discPower);
        },
        changeThrowType(event) {
            this.$emit('changeThrowType', this.discThrowType);
        }
    }
}
</script>
