<template>
    <headerFile />
    <sidebarFile :getTab="getComp"/>
    
    <div style="height: 830px;overflow-y: auto;">
        <br>
        <component :is="tab" />
    </div>

    <footerFile />
</template>

<script>
import headerFile from "./components/master/header-master.vue";
import sidebarFile from "./components/master/sidebar-master.vue";
import footerFile from "./components/master/footer-master.vue";
import indexFile from './components/index-cmp.vue';
import formFile from './components/form-cmp.vue';

export default {
    name: 'App',
    components: {
        headerFile,
        sidebarFile,
        footerFile,
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
