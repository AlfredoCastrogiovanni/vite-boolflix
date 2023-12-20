<script>
    import SingleMedia from './SingleMedia.vue';
    import { store } from '../js/store';

    export default {
    name: "ListMedia",
    data() {
        return {
            store
        }
    },
    components: {
        SingleMedia
    }
    }
</script>

<template>
    <div class="container-fluid">
        <div class="row justify-content-center">
            <SingleMedia v-for="media in store.mediaList" :media="media"/>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>