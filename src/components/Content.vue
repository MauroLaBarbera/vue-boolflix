<template>
    <div class="main-content">
        <Search @performSearch="filterFilm" />
        <div class="film-list">
            <ul v-for="film in filmList" :key="film.id">
                <li>Titolo: {{ film.title }}</li>
                <li>Titolo Originale: {{ film.original_title }}</li>
                <li>Lingua: {{ film.original_language }}</li>
                <li>Voto: {{ film.vote_average }}</li>
            </ul>
            <!-- <ul>
                <li v-for="film in filmList" :key="film.id">
                    <FilmList :info="film"/>
                </li>
            </ul> -->
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import Search from '@/components/Search.vue';
// import FilmList from '@/components/FilmList.vue';

export default {
    name: 'Components',
    components: {
        Search,
        //  FilmList,
    },
    data() {
        return {
            apiURL:
                'https://api.themoviedb.org/3/search/movie?api_key=6f56cfbbea1125659f4e05bff0a2ff1e&',
            filmList: [],
            loading: true,
            searchingFilms: 'all',
        };
    },
    created() {
        this.getFilm();
    },
    updated() {
        console.log(this.searchingFilms);
    },
    methods: {
        getFilm() {
            /**
             * Chiamata api
             */
            axios
                .get(this.apiURL, {
                    params: {
                        query: this.searchingFilms,
                    },
                })
                .then((res) => {
                    console.log(res.data.results);

                    this.filmList = res.data.results;
                    this.loading = false;
                })
                .catch((err) => {
                    console.log('Errore', err);
                });
        },

        filterFilm(text) {
            console.log('click', text);

            this.searchingFilms = text;
            this.getFilm();
            console.log(this.apiURL);
        },
    },
};
</script>

<style></style>
