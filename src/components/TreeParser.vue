<template>
    <v-layout>
        <v-flex>
            <v-textarea
                    label="Insert the JSON to parse into a tree view"
                    v-model="inputText">
            </v-textarea>
            <v-alert :value="parseError" color="error" outline>
                Error parsing the data. Please make sure that the data is in JSON format, and that all nodes have at
                least a "name" property.
            </v-alert>
            <v-btn class="ml-0 mt-3" color="success" @click.prevent="parseTree">Parse</v-btn>
        </v-flex>
    </v-layout>
</template>

<script>
export default {
    data() {
        return {
            inputText: '',
            parsedTree: [],
            parseError: false,
        };
    },
    methods: {
        parseTree() {
            try {
                this.parsedTree = JSON.parse(this.inputText);

                if (!this.checkTreeValid(this.parsedTree)) {
                    throw new Error('Tree has some invalid nodes.');
                }

                this.parseError = false;
                this.$emit('parsed', this.parsedTree);
            } catch (err) {
                this.parseError = true;
            }
        },
        checkTreeValid(tree) {
            return tree.reduce(
                (acc, curr) => (
                    acc
                    && curr.hasOwnProperty('name')
                    && (!curr.hasOwnProperty('children') || this.checkTreeValid(curr.children))
                ),
                true,
            );
        },
    },
};
</script>
