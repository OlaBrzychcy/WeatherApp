<template>
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
        <touchable-opacity :on-press="back" class="menu-button my-button"
                           v-if="data_bool && !weather_not_showed">
            <text class="text-field-title">Powrót</text>
        </touchable-opacity>

        <text class="text-field-title" v-if="current_weather">Temperature:{{data.current.temp}}</text>
        <text class="text-field-title" v-if="current_weather">Data:{{data.current.dt|moment}}</text>
    </view>
</template>
<script>
    import * as Location from "expo-location";
    import * as Permissions from "expo-permissions";
    import axios from 'axios';
    import moment from 'moment';


    export default {
        name: 'Content',
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
    .container {
        background-color: white;
        align-items: center;
        justify-content: center;
        flex: 1;
    }

    .text-color-primary {
        color: blue;
    }

    .my-button {
        background-color: lightgray;
        border-width: 1px;
        padding: 3%;
        border-radius: 10px;
    }

    .menu-button {
        margin: 2%;
    }
</style>
