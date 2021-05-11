<template>
    <div>
        <Content />
    </div>
</template>

<script>
import Content from '@/components/Content.vue';

export default {
    components: {
        Content,
    },
};
</script>

<style lang="scss">
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
}

body {
    background: #141414;
    color: #fff;
}
</style>
