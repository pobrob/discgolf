<template>
  <div>
      <select>
        <option v-for='vendor in vendors'>{{vendor}}
        </option>
      </select>
      <select>
        <option v-for='disc in discs'> {{disc.name}}
        </option>
      </select>
  </div>
</template>
<script>
export default {

    data() {
        return {
            vendors: ['Prodigy', 'Innova', 'Vibram'],
            discs: [
              {vendor: 'Innova', name: 'Avatar', type: 'putter', material: 'Championship'},
              {vendor: 'Innova', name: 'Destroyer', type: 'driver', material: 'Championship'},
              {vendor: 'Vibram', name: 'Lace', type: 'driver', material: 'X-Stiff'}
          ]
        }
    }
}
</script>

<style>

</style>
