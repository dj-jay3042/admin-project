<template>
    <!-- <headerFile /> -->
    <masterFile :getTab="getComp">
        <component :is="tab" />
    </masterFile>
    <!-- <footerFile /> -->
</template>

<script>
import masterFile from "./components/master/master-page.vue";
import indexFile from './components/index-cmp.vue';
import formFile from './components/form-cmp.vue';

export default {
    name: 'App',
    components: {
        masterFile,
        indexFile,
        formFile
    },
    data() {
        return {
            tab: "indexFile"
        }
    },
    methods: {
        getComp(tab) {
            this.tab = tab
        }
    }
}
</script>
