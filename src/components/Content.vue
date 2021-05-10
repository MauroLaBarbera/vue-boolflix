<template>
    <div class="main-content">
        <Search @performSearch="filterFilm" />
        <div class="film-list">
            <ul v-for="film in filmList" :key="film.id">
                <li>{{ film.title }}</li>
                <li>{{ film.original_title }}</li>
                <li>{{ film.original_language }}</li>
                <li>{{ film.vote_average }}</li>
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
                'https://api.themoviedb.org/3/search/movie?api_key=6f56cfbbea1125659f4e05bff0a2ff1e&query=ritorno al futuro',
            filmList: [],
            loading: true,
            searchingFilms: '',
        };
    },
    computed: {
        filteredFilm() {
            return this.filmList.filter((e) => {
                return e.original_title
                    .toLowerCase()
                    .includes(this.searchingFilms.toLowerCase());
            });
        },
    },
    created() {
        this.getFilm();
    },
    methods: {
        getFilm() {
            /**
             * Chiamata api
             */
            axios
                .get(this.apiURL)
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
        },
    },
};
</script>

<style></style>
