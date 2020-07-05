<template>
    <view class="main-container">
        <Header/>
        <Content/>
    </view>
</template>
<script>
    import Content from "./Content";
    import Header from "./Header";

    export default {
        name: 'Main',
        components: {Header, Content}
    }
</script>
<style scoped>
    .main-container {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
    }
</style>