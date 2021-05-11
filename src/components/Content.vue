<template>
    <div class="main-content">
        <Header @performSearch="filterFilm" />
        <div class="film-list">
            <!-- LISTA FILM -->
            <ul
                v-for="(film, index) in filmList"
                :key="film.id"
                @mouseenter="addClass(index)"
                @mouseleave="removeClass(index)"
            >
                <img
                    class="img"
                    :src="'https://image.tmdb.org/t/p/w342' + film.poster_path"
                    alt=""
                />

                <div class="info">
                    <div
                        :class="{ onhover: index === hover }"
                        v-if="hover === index"
                    >
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
                        <li>
                            Voto:
                            <i
                                v-for="(n, i) in Math.ceil(
                                    film.vote_average / 2
                                )"
                                :key="i"
                                class="fas fa-star"
                            ></i>
                            <i
                                v-for="(n, ind) in 5 -
                                    Math.ceil(film.vote_average / 2)"
                                :key="ind"
                                class="far fa-star"
                            ></i>
                        </li>
                        <li>Trama: {{ film.overview }}</li>
                    </div>
                </div>
            </ul>
            <!-- LISTA SERIE TV -->
            <ul
                v-for="series in seriesList"
                :key="series.id"
                @mouseenter="addClass(index)"
                @mouseleave="removeClass(index)"
            >
                <img
                    :src="
                        'https://image.tmdb.org/t/p/w342' + series.poster_path
                    "
                    alt=""
                />
                <div
                    :class="{ onhover: index === hover }"
                    v-if="hover === index"
                >
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
                    <li>
                        Voto:
                        <i
                            v-for="(n, i) in Math.ceil(series.vote_average / 2)"
                            :key="i"
                            class="fas fa-star"
                        ></i>
                        <i
                            v-for="(n, ind) in 5 -
                                Math.ceil(series.vote_average / 2)"
                            :key="ind"
                            class="far fa-star"
                        ></i>
                    </li>
                    <li>Trama: {{ series.overview }}</li>
                </div>
            </ul>
        </div>
    </div>
</template>

<script>
import Header from '@/components/Header.vue';
import axios from 'axios';

export default {
    name: 'Contents',
    components: {
        Header,
    },
    data() {
        return {
            apiURL:
                'https://api.themoviedb.org/3/search/movie?api_key=6f56cfbbea1125659f4e05bff0a2ff1e&language=it-IT',
            filmList: [],
            apiURL2:
                'https://api.themoviedb.org/3/search/tv?api_key=e99307154c6dfb0b4750f6603256716d&language=it-IT',
            seriesList: [],
            hover: null,
            searchingFilms: '',
        };
    },
    created() {
        this.getFilm();
        this.getSeries();
    },
    // updated() {
    //     console.log(this.searchingFilms);
    // },
    methods: {
        addClass(index) {
            this.hover = index;
            console.log(this.hover);
        },
        removeClass(index) {
            index = null;
            this.hover = index;
        },
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
                })
                .catch((err) => {
                    console.log('Errore', err);
                });
        },
        getVote() {
            axios.get(this.apiURL);
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
    margin-left: 5px;
}

i {
    margin-left: 5px;
}

.fas {
    color: goldenrod;
}

.far {
    color: goldenrod;
}

.film-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px;
}

.onhover {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: #000;
    overflow-y: scroll;
}

ul {
    position: relative;
    padding: 5px;
    margin: 10px;
    flex-basis: calc(100% / 6 - 20px);
    border: 1px solid #fff;
    border-radius: 5px;

    img {
        width: 100%;
        border-radius: 4px;
    }

    li {
        display: flex;
    }
}
</style>
