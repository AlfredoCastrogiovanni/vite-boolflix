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
        <div class="row justify-content-center" v-if="store.mediaList.length > 0">
            <SingleMedia v-for="media in store.mediaList" :key="media.id" :media="media"/>
        </div>
        <div class="row justify-content-center mt-5" v-else>
            <span class="text-center mt-5 fs-2">Search Movies & Series there!</span>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>