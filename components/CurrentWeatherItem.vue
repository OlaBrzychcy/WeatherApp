<template>
    <view class="container">
        <image class="img" :source="{uri: `${image_url}`}" style="max-width:100%;"/>
        <text class="text-field-title description">{{currentItem.weather[0].description|capital_first_letter}}</text>
        <text class="text-field-title right">Lokalizacja: {{latitude}}&deg{{hemisphere_NS}} {{longitude}}&deg{{hemisphere_EW}}</text>
        <text class="text-field-title right">Data: {{currentItem.dt|moment}}</text>
        <text class="text-field-title details">Temperatura: {{currentItem.temp}}&degC</text>
        <text class="text-field-title details">Temperatura odczuwalna: {{currentItem.feels_like}}&degC</text>
        <text class="text-field-title details">Ciśnienie: {{currentItem.pressure}} hPa</text>
        <text class="text-field-title details">Wilgotność: {{currentItem.humidity}}%</text>
        <text class="text-field-title details">Zachmurzenie: {{currentItem.clouds}}%</text>
        <text class="text-field-title details">Prędkość wiatru: {{currentItem.wind_speed}} m/s</text>

    </view>
</template>
<script>
    import moment from "moment";

    export default {
        name:'CurrentWeatherItem',
        props:['currentItem', 'latitude', 'longitude'],

        filters: {
            moment: function (date) {
                return moment.unix(date).format('DD-MM-YYYY HH:mm');
            },
            capital_first_letter: function (string) {
                return string.charAt(0).toUpperCase() + string.slice(1);
            }
        },
        computed:{
            image_url:function () {
                var url = `https://openweathermap.org/img/wn/${this.currentItem.weather[0].icon}@4x.png`;
                console.log(url);
                return url
            },
            hemisphere_NS:function () {
                if(this.longitude>0){
                    return 'N'
                }
                else{
                    return 'S'
                }
            },
            hemisphere_EW:function () {
                if(this.latitude>0){
                    return 'E'
                }
                else{
                    return 'W'
                }
            },


        }
    }
</script>
<style scoped>
    .container{
        border-width: 3px;
        border-color: rgba(0,0,0,0.2);
        padding: 2%;
        margin: 1%;
        max-width:320px;
        border-radius: 10px;
        flex: 1;

    }
    .img {
        height: 200px;
        width: 300px;
    }
    .description{
        text-align: center;
        font-weight: bold;
        font-size: 24px;
        padding: 1.5%;
    }
    .right{
        text-align: right;
        font-size: 10px;
    }
    .details{
        font-size: 16px;
        padding: 1%;
    }

</style>