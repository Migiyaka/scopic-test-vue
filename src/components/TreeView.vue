<template>
    <v-layout>
        <v-flex>
            <v-card>
                <v-toolbar card>
                    <v-toolbar-title>Tree View</v-toolbar-title>
                </v-toolbar>
                <v-layout>
                    <v-flex>
                        <v-card-text>
                            <template v-if="!items || items.length === 0">
                                <span>
                                    Tree items not found. Please use the text area above to parse some tree items.
                                </span>
                            </template>
                            <v-treeview
                                    :items="items"
                                    open-on-click
                                    selectable
                                    transition
                                    v-model="tree"
                                    item-key="name"
                                    expand-icon="mdi-chevron-down"
                                    :indeterminate-icon="$vuetify.icons.checkboxOn"></v-treeview>
                        </v-card-text>
                    </v-flex>
                    <v-divider vertical></v-divider>
                    <v-flex xs6>
                        <v-card-text>
                            <h4 class="title font-weight-light grey--text pb-3">
                                Selected tree nodes
                            </h4>
                            <span>{{ treeTextView }}</span>
                        </v-card-text>
                    </v-flex>
                </v-layout>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>
export default {
    props: ['items'],
    data() {
        return {
            tree: [],
        };
    },
    computed: {
        treeTextView() {
            let nodesToHide = [];

            this.tree.forEach((node) => {
                const nodeToCheck = this.findNode(node, this.items);

                if (this.allChildrenSelected(nodeToCheck)) {
                    nodesToHide = nodesToHide.concat(nodeToCheck.children.map(child => child.name));
                }
            });

            const nodesToShow = this.tree.filter(node => nodesToHide.indexOf(node) < 0);

            return this.tree && this.tree.length > 0 ? nodesToShow.join(', ') : '';
        },
    },
    methods: {
        findNode(name, tree) {
            if (tree.find(node => node.name === name)) {
                return tree.find(node => node.name === name);
            } else {
                return tree.reduce((acc, curr) => {
                    if (curr.hasOwnProperty('children')) {
                        return acc || this.findNode(name, curr.children);
                    }

                    return acc || false;
                }, false);
            }
        },
        allChildrenSelected(node) {
            return node.hasOwnProperty('children')
                && node.children.reduce((acc, curr) => acc && this.tree.indexOf(curr.name) >= 0, true);
        },
    },
};
</script>
