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
        <div class="row justify-content-center">
            <SingleMovie v-for="movie in store.movieList" :movie="movie"/>
            <SingleTv v-for="tv in store.tvList" :tv="tv"/>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>