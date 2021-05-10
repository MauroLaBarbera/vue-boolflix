<template>
    <div class="main-content">
        <Search @performSearch="filterFilm" />
        <div class="film-list">
            <ul v-for="film in filmList" :key="film.id">
                <li>Titolo: {{ film.title }}</li>
                <li>Titolo Originale: {{ film.original_title }}</li>
                <li>
                    Lingua:
                    <img
                        class="flag"
                        v-if="film.original_language === 'en'"
                        src="../assets/img/en.png"
                    />
                    <img
                        class="flag"
                        v-else-if="film.original_language === 'it'"
                        src="../assets/img/it.png"
                    />
                    <span v-else>{{ film.original_language }}</span>
                </li>
                <li>Voto: {{ film.vote_average }}</li>
            </ul>
            <ul v-for="series in seriesList" :key="series.id">
                <li>Titolo: {{ series.name }}</li>
                <li>Titolo Originale: {{ series.original_name }}</li>
                <li>
                    Lingua:
                    <img
                        class="flag"
                        v-if="series.original_language === 'en'"
                        src="../assets/img/en.png"
                    />
                    <img
                        class="flag"
                        v-else-if="series.original_language === 'it'"
                        src="../assets/img/it.png"
                    />
                    <span v-else>{{ series.original_language }}</span>
                </li>
                <li>Voto: {{ series.vote_average }}</li>
            </ul>
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
                'https://api.themoviedb.org/3/search/movie?api_key=6f56cfbbea1125659f4e05bff0a2ff1e&language=it-IT',
            filmList: [],
            apiURL2:
                'https://api.themoviedb.org/3/search/tv?api_key=e99307154c6dfb0b4750f6603256716d&language=it-IT',
            seriesList: [],
            loading: true,
            searchingFilms: '',
        };
    },
    created() {
        this.getFilm();
        this.getSeries();
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
        getSeries() {
            /**
             * Chiamata api
             */
            axios
                .get(this.apiURL2, {
                    params: {
                        query: this.searchingFilms,
                    },
                })
                .then((res) => {
                    console.log(res.data.results);

                    this.seriesList = res.data.results;
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
            this.getSeries();
            console.log(this.apiURL);
        },
    },
};
</script>

<style scoped lang="scss">
.flag {
    width: 20px;
    height: 15px;
}
</style>
