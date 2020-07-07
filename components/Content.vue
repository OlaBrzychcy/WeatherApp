<template>

    <view class="container">
        <view class="content-header-container" v-if="data_bool && current_weather && !weather_not_showed">
            <touchable-opacity :on-press="back" class="back-button">
                <text><<</text>
            </touchable-opacity>
            <text class="header">Aktulana Pogoda</text>
        </view>
        <view class="content-header-container" v-if="data_bool && hourly_weather && !weather_not_showed">
            <touchable-opacity :on-press="back" class="back-button">
                <text class="text-field-title"><<</text>
            </touchable-opacity>
            <text class="header">Prognoza Godzinowa</text>
        </view>
        <view class="content-header-container" v-if="data_bool && daily_weather && !weather_not_showed">
            <touchable-opacity :on-press="back" class="back-button">
                <text class="text-field-title"><<</text>
            </touchable-opacity>
            <text class="header">Prognoza dzienna</text>
        </view>
        <view class="loading-container" v-if="!data_bool">
            <activity-indicator size="large" color="#0000ff"></activity-indicator>
        </view>
        <view class="container">
        <touchable-opacity :on-press="showCurrentWeather" class="menu-button my-button"
                           v-if="data_bool && weather_not_showed">
            <text class="text-field-title">Aktualna Pogoda</text>
        </touchable-opacity>
        <touchable-opacity :on-press="showHourlyWeather" class="menu-button my-button"
                           v-if="data_bool && weather_not_showed">
            <text class="text-field-title">Prognoza Godzinowa</text>
        </touchable-opacity>
        <touchable-opacity :on-press="showDailyWeather" class="menu-button my-button"
                           v-if="data_bool && weather_not_showed">
            <text class="text-field-title">Prognoza Dzienna</text>
        </touchable-opacity>

        </view>

        <scroll-view class="scroll-view">



        <CurrentWeatherItem :current-item="data.current" :latitude="latitude" :longitude="longitude" v-if="current_weather"/>


        <CurrentWeatherItem :current-item="item" :latitude="latitude" :longitude="longitude" v-if="hourly_weather" v-for="item in data.hourly"/>

        <DailyWeatherItem :current-item="item" :latitude="latitude" :longitude="longitude" v-if="daily_weather" v-for="item in data.daily"/>
            </scroll-view>
    </view>
</template>
<script>
    import * as Location from "expo-location";
    import * as Permissions from "expo-permissions";
    import axios from 'axios';
    import moment from 'moment';
    import CurrentWeatherItem from "./CurrentWeatherItem";
    import DailyWeatherItem from "./DailyWeatherItem";


    export default {
        name: 'Content',
        components: {CurrentWeatherItem,DailyWeatherItem},
        data: function () {
            return {
                location: {},
                latitude: "",
                longitude: "",
                errorMessage: "",
                API_key: '57f44175e5284473edbdc5b9138c8a36',
                data: {},
                data_bool: false,
                current_weather: false,
                hourly_weather: false,
                daily_weather: false,
                weather_not_showed: true,

            };
        },
        created() {
            this.getLocationWeather();
        },
        filters: {
            moment: function (date) {
                return moment(date).format('DD-MM-YYYY HH:mm');
            }
        },

        methods: {

            getLocationWeather: function () {
                this.data_bool = false;
                Permissions.askAsync(Permissions.LOCATION)
                    .then(status => {
                        if (!status.granted) {
                            this.errorMessage = "Permission to access location was denied";
                        } else if (status.granted) {
                            Location.getCurrentPositionAsync({}).then(location => {
                                this.location = location;
                                console.log(location);
                                this.latitude = location.coords.latitude
                                this.longitude = location.coords.longitude
                                this.errorMessage = "";
                                this.getWeather(this.latitude, this.longitude)
                            });
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    });
            },
            getWeather: function (latitude, longitude) {
                var url = `https://api.openweathermap.org/data/2.5/onecall?lat=${latitude}&lon=${longitude}&units=metric&lang=pl&exclude=minutely&appid=${this.API_key}`
                return axios.get(url).then((response) => {
                    this.data = response.data;
                    this.data_bool = true;
                }).catch(function (error) {
                    throw error;
                })
            },
            showCurrentWeather: function () {

                this.current_weather = true
                this.daily_weather = false
                this.hourly_weather = false
                this.weather_not_showed = false
            },
            showHourlyWeather: function () {
                this.current_weather = false
                this.daily_weather = false
                this.hourly_weather = true
                this.weather_not_showed = false
            },
            showDailyWeather: function () {
                this.current_weather = false
                this.daily_weather = true
                this.hourly_weather = false
                this.weather_not_showed = false
            },
            back:function () {
                this.current_weather = false
                this.daily_weather = false
                this.hourly_weather = false
                this.weather_not_showed = true
            }
        }
    };
</script>


<style scoped>
    .loading-container{
        flex: 1;
        justifyContent: center;
        height: 100%;
        width: 100%;
    }
    .container {
        background-color: white;
        align-items: center;
        justify-content: center;
        display: flex;
        flex-direction: column;
        flex: 1;
    }
    .content-header-container {

        flex-direction: row;
        width: 100%;
        max-width: 320px;
        background-color: rgb(255,255,255);
        display: flex;
        border-bottom-color: lightgray;
        border-bottom-width: 1px;

    }
    .header {
        font-size: 16px;
        color: black;
        opacity: 0.8;
        font-weight: 600;
        text-align: center;
        padding-top: 15px;
        padding-bottom: 15px;

    }
    .back-button{
        padding: 15px;
        font-weight: bold;
        color: lightgray;


    }





    .text-color-primary {
        color: blue;
    }

    .my-button {
        background-color: white;
        color:lightgray;
        border-width: 3px;
        border-color: lightgray;
        padding: 3%;
        border-radius: 10px;
    }

    .menu-button {
        margin: 2%;
    }
</style>
