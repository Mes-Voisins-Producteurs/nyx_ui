<template>
  <el-dialog
    width="80%"
    :title="title"
    :visible.sync="dialogFormVisible"
    :before-close="closeDialog"
    :close-on-click-modal="false"
    class="purchase-order-editor"
  >

   
    purchase order


    <span slot="footer" class="dialog-footer">
      <el-button @click="$emit('dialogclose')">{{this.$t("buttons.quit")}}</el-button>
    </span>
  </el-dialog>
</template>

<script>
import Vue from "vue";

import YAML from "js-yaml";
import axios from "axios";

export default {
  name: "poEditor",
  data: () => ({
    orgRec: null,
    newRec: null,
    formLabelWidth: "120px",
    changed: false,
    dialogFormVisible: false,
    title: "My super title",
    
  }),
  computed: {
    recordin: function() {
      return this.record;
    },
    recordstr: function() {
      return JSON.stringify(this.record);
    },
    recchanged: function() {
      return JSON.stringify(this.recordin) != JSON.stringify(this.newRec);
    }
  },
  props: {
    record: {
      type: Object
    },
    config: {
      type: Object
    },
    editMode: {
      type: String
    }
  },
  watch: {
    recordin: {
      handler: function() {
        
      },
      deep: true
    }
  },
  created: function() {
    this.dialogFormVisible = true;

    console.log(this.record)
  },
  components: {},
  methods: {
    closeDialog: function() {
      this.$emit("dialogclose");
    },
  }
};
</script>

<style >

</style>