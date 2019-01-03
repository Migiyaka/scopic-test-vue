<template>
    <v-app>
        <v-content>
            <v-container>
                <h1>Scopic Vue Test</h1>
                <tree-wrapper></tree-wrapper>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
import TreeWrapper from './components/TreeWrapper.vue';

export default {
    name: 'App',
    components: {
        TreeWrapper,
    },
};
</script>
