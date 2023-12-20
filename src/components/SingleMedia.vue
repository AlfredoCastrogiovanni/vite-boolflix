<script>
    import LangFlag from '../../node_modules/vue-lang-code-flags'

    export default {
    name: "SingleMedia",
    data() {
        return {
            
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
    <div class="card col-2 mb-3 me-3">
        <img :src="(media.backdrop_path != null) ? `http://image.tmdb.org/t/p/w500/${media.backdrop_path}` : '/placeholder.jpg' " class="card-img-top mt-2" alt="...">
        <div class="card-body">
            <h5 class="card-title">{{ media.title }}</h5>
        </div>
        <ul class="list-group list-group-flush">
            <li class="list-group-item">Titolo originale: {{ media.original_title }} </li>
            <li class="list-group-item">Lingua: <lang-flag :iso="media.original_language"/> </li>
            <li class="list-group-item">Voto: {{ getStars(media.vote_average) }} </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>

</style>