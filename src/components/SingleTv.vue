<script>
    import LangFlag from '../../node_modules/vue-lang-code-flags';
    import axios from 'axios';

    export default {
    name: "SingleTv",
    data() {
        return {
            currentTv: []
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
        getDetails(id) {
            axios.get(`https://api.themoviedb.org/3/tv/${id}`, {
                params: {
                api_key: '5b642de1640631e7141e3df914a0816c',
                language: 'it-IT',
                append_to_response: 'credits'
                }
            })
            .then( response => {
                console.log(response);
                this.currentTv = response.data;
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
        tv: {
            type: Object,
            required: true
        }
    }
    }
</script>

<template>
    <article class="col-12 col-md-3 col-lg-2 col-xxl-2 mb-3 me-md-3 my_card p-md-0">
        <div class="my_imgWrapper" data-bs-toggle="modal" :data-bs-target="`#staticBackdrop${tv.id}`" @mouseup="getDetails(tv.id)">
            <img :src="(tv.poster_path != null) ? `http://image.tmdb.org/t/p/w342/${tv.poster_path}` : '/placeholder.jpg' " alt="...">
        </div>
    
        <!-- Modal -->
        <div class="modal fade" :id="`staticBackdrop${tv.id}`" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="staticBackdropLabel">{{ tv.name }}</h1>
            </div>
            <div class="modal-body">
                <ul class="list-group">
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Titolo originale</div>
                        {{ tv.original_name }}
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Lingua</div>
                        <lang-flag :iso="tv.original_language"/>
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Voto</div>
                        {{ getStars(tv.vote_average) }}
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start" v-if="currentTv != 0">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Cast</div>
                        <ol class="ps-3">
                            <li v-for="i in 5">{{ currentTv.credits.cast[i].name }}</li>
                        </ol>

                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start" v-if="currentTv != 0">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Genres</div>
                        <ol class="ps-3">
                            <li v-for="genre in currentTv.genres">{{ genre.name }}</li>
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