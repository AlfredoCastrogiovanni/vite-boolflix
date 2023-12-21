<script>
    import LangFlag from '../../node_modules/vue-lang-code-flags';
    import axios from 'axios';

    export default {
    name: "SingleMedia",
    data() {
        return {
            currentMedia: []
        }
    },
    methods: {
        getStars(vote) {
            let stars = ""
            const starNumber = parseInt(vote / 2);
            for(let i=0; i < starNumber; i++) {
                stars += "★";
            }
            for(let i=0; i < 5 - starNumber; i++) {
                stars += "☆";
            }
            return stars;
        },
        getMovieDetails(id) {
            axios.get(`https://api.themoviedb.org/3/movie/${id}`, {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                append_to_response: 'credits'
                }
            })
            .then( response => {
                this.currentMedia = response.data;
            })
            .catch(function (error) {
                console.log(error);
            }); 
        },
        getTvDetails(id) {
            axios.get(`https://api.themoviedb.org/3/tv/${id}`, {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                append_to_response: 'credits'
                }
            })
            .then( response => {
                this.currentMedia = response.data;
            })
            .catch(function (error) {
                console.log(error);
            }); 
        }
    },
    components: {
        LangFlag
    },
    props: {
        media: {
            type: Object,
            required: true
        }
    }
    }
</script>

<template>
    <article class="col-12 col-md-6 col-lg-4 col-xxl-2 mb-3 me-3 my_card p-0">
        <div class="my_imgWrapper" data-bs-toggle="modal" :data-bs-target="`#staticBackdrop${media.id}`" @mouseup="media.title ? getMovieDetails(media.id) : getTvDetails(media.id)">
            <img :src="(media.poster_path != null) ? `http://image.tmdb.org/t/p/w342/${media.poster_path}` : '/placeholder.jpg' " alt="...">
        </div>
    
        <!-- Modal -->
        <div class="modal fade" :id="`staticBackdrop${media.id}`" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="staticBackdropLabel">{{ media.title ? media.title : media.name }}</h1>
            </div>
            <div class="modal-body">
                <ul class="list-group">
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Titolo originale</div>
                        {{ media.original_title ? media.original_title : media.original_name }}
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Lingua</div>
                        <lang-flag :iso="media.original_language"/>
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Voto</div>
                        {{ getStars(media.vote_average) }}
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start" v-if="currentMedia != 0">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Cast</div>
                        <ol class="ps-3">
                            <li v-for="i in (currentMedia.credits.cast.length > 5 ? 5 : currentMedia.credits.cast.length - 1)">{{ currentMedia.credits.cast[i].name }}</li>
                        </ol>

                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start" v-if="currentMedia != 0">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Genres</div>
                        <ol class="ps-3">
                            <li v-for="genre in currentMedia.genres">{{ genre.name }}</li>
                        </ol>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-danger" data-bs-dismiss="modal">Close</button>
            </div>
            </div>
        </div>
        </div>
    </article>
</template>

<style lang="scss" scoped>

</style>