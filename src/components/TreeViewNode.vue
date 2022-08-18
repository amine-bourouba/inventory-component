<template>
  <div>
    <div v-if="item">
      <div
        class="mb-2"
        :class="{ 'has-children': item.children.length }"
        @click="toggleChildren"
      >
        <span class="mr-2">{{ toggleIcon }}</span>
        <span class="mr-2">{{ item.name }}</span>
        <span
          v-if="item.discoveredIssues > 0"
          class="rounded-xl bg-red-600 text-white px-2"
        >
          {{ item.discoveredIssues }}
        </span>
        <span v-if="item.discoveredIssues > 0" class="text-red-600 px-2">
          {{ getTotalIssueCount(item) }}
        </span>
      </div>
      <div v-if="item.children.length > 0 && showChildren" class="ml-10">
        <tree-view-node
          v-for="item in item.children"
          :key="item.id"
          :item="item"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TreeViewNode from "./TreeViewNode.vue";
export default {
  name: "TreeViewNode",
  components: { TreeViewNode },
  props: {
    item: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      showChildren: false,
    };
  },
  computed: {
    toggleIcon() {
      if (this.showChildren) {
        return this.item.children.length > 0 ? "-" : ""
      } else {
        return this.item.children.length > 0 ? "+" : ""
      }
    }
  },
  methods: {
    toggleChildren() {
      this.showChildren = !this.showChildren;
    },
    getTotalIssueCount(node) {
      if (node.children.length <= 0) {
        return null;
      } else {
        let total = 0;
        node.children.forEach((child) => {
          total += child.discoveredIssues + this.getTotalIssueCount(child)
        });
        return total;
      }
    },
  },
};
</script>

<style scoped>
.has-children {
  cursor: pointer;
}
</style>