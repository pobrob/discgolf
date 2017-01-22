<template lang="html">
  <div class="container ">
    <div class="panel panel-default">
      <div class="panel-heading">
        <slot></slot>
      </div>
      <div class="panel-body">
        <ul class="list-group">
            <li  v-for="disc in discs" class="list-group-item disc">
              <input type="checkbox"/>
              {{disc.disc}}
              <select v-model:value='disc.wear'>
                <option v-for="level in wearLevels" :value='level'>{{level}}/10</option>
              </select>
              <button @click="removeDisc(disc)">Remove</button>
            </li>
          </ul>
      </div>
    </div>
    <div>
      <button @click="addDiscToBag();" style="float:right;">Add to bag</button>
      <select style="width:76%;float:left;" id="disc" v-model="selectedDisc">
      <option v-for='(discInstance, discIndex) in filteredDiscs' :value="discInstance"  >{{discInstance.disc}}</option></select>
      <div style="clear:both;"></div>
    </div>
    <importExport></importExport>
</div>
</template>

<script>
import ImportExport from './ImportExport.vue';
export default {
    data() {
        return {
            wearLevels: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
            wear: 10,
            discdata: [],
            selectedDisc: {},
            discFilter: ''
        }
    },
    props: ['discs'],
    methods: {
        removeDisc(disc) {
            this.discs.splice(this.discs.indexOf(disc), 1);
            this.updateBag();
            alert("removed" + disc.disc);
        },
        addDiscToBag() {
            //var d = $("#disc").val().split(';');
            //var dn = $("#disc option:selected").html();
            //addDisc(d, dn, 10, true);
            var addedDisc = {wear: 10,
            enabled: true,
          data: this.selectedDisc.data.split(';'),
            disc: this.selectedDisc.disc};

            this.discs.push(addedDisc);
            this.updateBag();
        },
        containsFilter(value) {
            return value.disc.toLowerCase().indexOf(this.discFilter.toLowerCase()) > -1;
        },
        updateBag() {
            if (typeof(Storage) !== "undefined") {
                localStorage["bag"] = JSON.stringify(this.discs);
            }
            //updateCanvas();
            //return bag;
        }
    },
    computed: {
        filteredDiscs() {
            return this.discdata.filter(this.containsFilter)
        }
    },
    mounted() {
        var self = this;
        $.getJSON("./discdata.json", function(json) {
            self.discdata = json;
        });
    },
    components : {
      'importExport': ImportExport
    }
}
</script>

<style lang="css" scoped>
  .disc {text-align: left;}

</style>
