<script>
    import { store } from '../js/store';
    import axios from 'axios';

    export default {
    name: "AppHeader",
    data() {
        return {
            store,
            query: '',
            movieGenreList: [],
            tvGenreList: [],
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
                response.data.results.forEach(element => {
                    this.store.mediaList.push(element)
                });
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
                response.data.results.forEach(element => {
                    this.store.mediaList.push(element)
                });
                this.query = "";
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        movieDiscovery(with_genres = '') {
            axios.get('https://api.themoviedb.org/3/discover/movie', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                sort_by: 'popularity.desc',
                with_genres
                }
            })
            .then( response => {
                this.store.mediaList = response.data.results;
                this.query = "";
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        tvDiscovery(with_genres = '') {
            axios.get('https://api.themoviedb.org/3/discover/tv', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                sort_by: 'popularity.desc',
                with_genres
                }
            })
            .then( response => {
                this.store.mediaList = response.data.results;
                this.query = "";
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        trendingMovie() {
            axios.get('https://api.themoviedb.org/3/trending/movie/day', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                }
            })
            .then( response => {
                if(this.store.mediaList.length <= 0) {
                    response.data.results.forEach(element => {
                        this.store.mediaList.push(element)
                    });
                } else {
                    this.store.mediaList = response.data.results;
                }
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        trendingTv() {
            axios.get('https://api.themoviedb.org/3/trending/tv/day', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                }
            })
            .then( response => {
                response.data.results.forEach(element => {
                    this.store.mediaList.push(element)
                });
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        getMovieGenres() {
            axios.get('https://api.themoviedb.org/3/genre/movie/list', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                }
            })
            .then( response => {
                this.movieGenreList = response.data.genres;
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        getTvGenres() {
            axios.get('https://api.themoviedb.org/3/genre/tv/list', {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                }
            })
            .then( response => {
                this.tvGenreList = response.data.genres;
            })
            .catch(function (error) {
                console.log(error);
            }); 
        }
    },
    created() {
        this.trendingMovie();
        this.trendingTv();
        this.getMovieGenres();
        this.getTvGenres();
    }
    }
</script>

<template>
    <nav class="navbar navbar-expand-lg bg-body-tertiary mb-4" data-bs-theme="dark">
        <div class="container-fluid">
            <a class="navbar-brand text-danger fw-bold fs-2 text-uppercase">BoolFlix</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#" @click="this.trendingMovie(), this.trendingTv()">Home</a>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false" data-bs-auto-close="outside">
                            Discovery
                        </a>
                        <ul class="dropdown-menu">
                            <li class="dropend">
                                <a class="dropdown-item dropdown-toggle" data-bs-toggle="dropdown" href="#" @click="movieDiscovery()">Movies</a>
                                <ul class="dropdown-menu">
                                    <li v-for="genre in movieGenreList" :key="genre.id"><a class="dropdown-item" href="#" @click="movieDiscovery(genre.id)">{{ genre.name }}</a></li>
                                </ul>
                            </li>
                            <li class="dropend">
                                <a class="dropdown-item dropdown-toggle" data-bs-toggle="dropdown" href="#" @click="tvDiscovery()">Series TV</a>
                                <ul class="dropdown-menu">
                                    <li v-for="genre in tvGenreList" :key="genre.id"><a class="dropdown-item" href="#" @click="tvDiscovery(genre.id)">{{ genre.name }}</a></li>
                                </ul>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
            <div class="d-flex" role="search">
                <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" v-model="query" @keydown.enter="getMovie(query), getTv(query)">
                <button class="btn btn-danger" @click="getMovie(query), getTv(query)">Search</button>
            </div>
        </div>
    </nav>
</template>

<style lang="scss" scoped>

</style>