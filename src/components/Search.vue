<template>
    <div class="search">
        <div class="search-bar">
            <input type="text" placeholder="Search" v-model.trim="searchText" />
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
</template>

<script>
export default {
    name: 'Search',
    data() {
        return {
            searchText: '',
        };
    },
};
</script>

<style scoped lang="scss"></style>
