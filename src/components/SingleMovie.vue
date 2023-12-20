<script>
    import LangFlag from '../../node_modules/vue-lang-code-flags'

    export default {
    name: "SingleMovie",
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

        }
    },
    components: {
        LangFlag
    },
    props: {
        movie: {
            type: Object,
            required: true
        }
    }
    }
</script>

<template>
    <article class="col-12 col-md-6 col-lg-4 col-xxl-2 mb-3 me-3 my_card p-0">
        <div class="my_imgWrapper" data-bs-toggle="modal" :data-bs-target="`#staticBackdrop${movie.id}`">
            <img :src="(movie.poster_path != null) ? `http://image.tmdb.org/t/p/w342/${movie.poster_path}` : '/placeholder.jpg' " alt="...">
        </div>
    
        <!-- Modal -->
        <div class="modal fade" :id="`staticBackdrop${movie.id}`" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="staticBackdropLabel">{{ movie.title }}</h1>
            </div>
            <div class="modal-body">
                <ul class="list-group">
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Titolo originale</div>
                        {{ movie.original_title }}
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Lingua</div>
                        <lang-flag :iso="movie.original_language"/>
                        </div>
                    </li>
                    <li class="list-group-item d-flex justify-content-between align-items-start">
                        <div class="ms-2 me-auto">
                        <div class="fw-bold text-danger">Voto</div>
                        {{ getStars(movie.vote_average) }}
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