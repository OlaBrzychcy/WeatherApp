<template>
    <view class="header-container">
        <text class="header">Weather App</text>
    </view>
</template>

<script>
    export default {
        name: 'Header'
    }
</script>
<style scoped>
    .header-container {
        background-color: rgb(255,255,255);
        display: flex;
        justify-content: center;
        padding-top: 15px;
        padding-bottom: 15px;
        border-bottom-color: lightgray;
        border-bottom-width: 1px;
        margin-top: 25px;

    }
    .header {
        font-size: 16px;
        color: black;
        opacity: 0.8;
        font-weight: 600;
        text-align: center;
    }
</style>