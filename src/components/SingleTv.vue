<script>
    import LangFlag from '../../node_modules/vue-lang-code-flags'

    export default {
    name: "SingleTv",
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
        tv: {
            type: Object,
            required: true
        }
    }
    }
</script>

<template>
    <div class="card col-2 mb-3 me-3">
        <img :src="(tv.poster_path != null) ? `http://image.tmdb.org/t/p/w342/${tv.poster_path}` : '/placeholder.jpg' " class="card-img-top mt-2" alt="...">
        <div class="card-body">
            <h5 class="card-title">{{ tv.name }}</h5>
        </div>
        <ul class="list-group list-group-flush">
            <li class="list-group-item">Titolo originale: {{ tv.original_name }} </li>
            <li class="list-group-item">Lingua: <lang-flag :iso="tv.original_language"/> </li>
            <li class="list-group-item">Voto: {{ getStars(tv.vote_average) }} </li>
        </ul>
    </div>
</template>

<style lang="scss" scoped>

</style>