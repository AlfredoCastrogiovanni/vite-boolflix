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
        getMovie(query) {
            axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                query,
                language: 'it-IT'
                }
            })
            .then( response => {
                console.log(response);
                this.store.movieList = response.data.results;
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        getTv(query) {
            axios.get('https://api.themoviedb.org/3/search/tv', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                query,
                language: 'it-IT'
                }
            })
            .then( response => {
                console.log(response);
                this.store.tvList = response.data.results;
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
    <nav class="navbar bg-body-tertiary mb-4" data-bs-theme="dark">
        <div class="container-fluid">
            <a class="navbar-brand text-danger fw-bold fs-2 text-uppercase">BoolFlix</a>
            <div class="d-flex" role="search">
                <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" v-model="query" @keydown.enter="getMovie(query), getTv(query)">
                <button class="btn btn-danger" @click="getMovie(query), getTv(query)">Search</button>
            </div>
        </div>
    </nav>
</template>

<style lang="scss" scoped>

</style>