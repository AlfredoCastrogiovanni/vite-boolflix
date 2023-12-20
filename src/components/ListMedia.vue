<script>
    import SingleMovie from './SingleMovie.vue';
    import SingleTv from './SingleTv.vue';
    import { store } from '../js/store';

    export default {
    name: "ListMedia",
    data() {
        return {
            store
        }
    },
    components: {
        SingleMovie,
        SingleTv
    }
    }
</script>

<template>
    <div class="container-fluid">
        <div class="row justify-content-center" v-if="store.movieList.length > 0 || store.tvList.length > 0">
            <SingleMovie v-for="movie in store.movieList" :key="movie.id" :movie="movie"/>
            <SingleTv v-for="tv in store.tvList" :key="tv.id" :tv="tv"/>
        </div>
        <div class="row justify-content-center mt-5" v-else>
            <span class="text-center mt-5 fs-2">Search Movies & Series there!</span>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>