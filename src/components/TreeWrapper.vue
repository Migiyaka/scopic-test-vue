<template>
    <v-container>
        <tree-parser class="mb-5" @parsed="treeParsed"></tree-parser>
        <tree-view :items="treeItems"></tree-view>
    </v-container>
</template>

<script>
import TreeParser from './TreeParser.vue';
import TreeView from './TreeView.vue';

export default {
    components: {
        TreeParser,
        TreeView,
    },
    data() {
        return {
            treeItems: [],
        };
    },
    methods: {
        treeParsed(parsedItems) {
            this.treeItems = parsedItems;
        },
    },
};
</script>
