<template>
    <header>
        <div class="logo">
            <img
                src="https://fontmeme.com/permalink/210511/d0a022572e97a880d85d1b984025088c.png"
                alt=""
            />
        </div>
        <!-- <Search @performSearch="filterFilm" /> -->
        <div class="search">
            <div class="search-bar">
                <input
                    type="text"
                    placeholder="Search"
                    v-model.trim="searchText"
                    @keyup="$emit('performSearch', searchText)"
                />
            </div>
            <div class="button">
                <button
                    type="submit"
                    @click.prevent="$emit('performSearch', searchText)"
                >
                    Search
                </button>
            </div>
        </div>
    </header>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Header',
    components: {},
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
            searchText: '',
        };
    },
    created() {
        this.getFilm();
        this.getSeries();
    },
    updated() {
        // console.log(this.searchingFilms);
        console.log('Array film', this.filmList);
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
                    //console.log(res.data.results);

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
                    //console.log(res.data.results);

                    this.seriesList = res.data.results;
                    this.loading = false;
                })
                .catch((err) => {
                    console.log('Errore', err);
                });
        },
        // getVote() {
        //     axios
        //     .get(this.apiURL)
        // },

        filterFilm(text) {
            //console.log('click', text);

            this.searchingFilms = text;
            this.getFilm();
            this.getSeries();
            //console.log(this.apiURL);
        },
    },
};
</script>

<style scoped lang="scss">
header {
    background: #000;
    display: flex;
    justify-content: space-between;
    .logo {
        img {
            width: 150px;
            padding: 10px;
        }
    }
}

.search {
    display: flex;
    align-items: center;
}

.button {
    margin: 0 10px;
}

input,
button {
    outline: none;
    background: #000;
    color: #fff;
    border: none;
    cursor: pointer;
}

input {
    border-bottom: 1px solid #fff;
    padding: 5px;
    transition: background 0.3s;

    &:focus {
        background: rgba(#fff, 0.1);
    }
}

button {
    border: 1px solid #fff;
    padding: 7px;
    border-radius: 5px;
    transition: background 0.3s;

    &:hover {
        background: #db1f2c;
        color: rgba(#000, 0.8);
        border: #db1f2c;
    }
}
</style>
