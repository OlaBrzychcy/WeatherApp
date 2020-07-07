<!--Main Component stores Header and Content component-->
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
    .scroll-view {

        /*padding-top: 20px;*/
        /*padding-bottom: 30px;*/
        /*margin: 1%;*/
        flex:1;

    }
    .container{
        background-color: white;
        align-items: center;
        justify-content: center;
        display: flex;
        flex-direction: column;
        flex: 1;
    }
    .main-container {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;

    }
</style>