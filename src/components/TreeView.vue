<template>
  <div>
    <div v-if="inventoryData.resultDetails.length > 0">
      <tree-view-node 
        v-for="item in inventoryData.resultDetails" 
        :key="item.id"
        :item="item"
      />
    </div>
    <div v-else>
      <!-- NO DATA ALERT -->
    </div>
  </div>
</template>

<script>
import TreeViewNode from "./TreeViewNode.vue";
export default {
  name: "TreeView",
  components: {
    TreeViewNode,
  },
  data() {
    return {
      inventoryData: require("./tree.json"),
    };
  },
};
</script>