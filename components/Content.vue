<template>
    <view class="container">
        <text class="text-field-title">Location Object:</text>
        <text>{{ location }}</text>
        <text class="text-field-title">Latitude Only:</text>
        <text>{{ latitude }}</text>
        <text class="text-field-title">Longitude Only:</text>
        <text>{{ longitude }}</text>
        <text class="text-error">{{ errorMessage }}</text>
    </view>
</template>
<script>
    import * as Location from "expo-location";
    import * as Permissions from "expo-permissions";
    import axios from 'axios';


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
                data_bool: false

            };
        },
        created() {
            this.getLocationWeather();
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
</style>
