<script>
    import { store } from '../js/store';
    import axios from 'axios';

    export default {
    name: "AppHeader",
    data() {
        return {
            store,
            query: '',
        }
    },
    methods: {
        getMedia(query) {
            axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                query,
                language: 'it-IT'
                }
            })
            .then( response => {
                console.log(response);
                this.store.mediaList = response.data.results;
                this.query = "";
            })
            .catch(function (error) {
                console.log(error);
            }); 
        }
    }
    }
</script>

<template>
    <nav class="navbar bg-body-tertiary mb-4">
        <div class="container-fluid">
            <a class="navbar-brand text-danger fw-bold fs-2">BoolFlix</a>
            <div class="d-flex" role="search">
                <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" v-model="query">
                <button class="btn btn-danger" @click="getMedia(query)">Search</button>
            </div>
        </div>
    </nav>
</template>

<style lang="scss" scoped>

</style>