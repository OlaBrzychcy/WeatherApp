<template>
    <view class="container">
        <touchable-opacity :on-press="getLocation" class="my-button">
            <text class="text-field-title">Get Location</text>
        </touchable-opacity>
        <text class="text-field-title" v-if="has_data">Location Object:</text>
        <text v-if="has_data">{{ location }}</text>
        <text class="text-field-title" v-if="has_data">Latitude Only:</text>
        <text v-if="has_data">{{ latitude }}</text>
        <text class="text-field-title" v-if="has_data">Longitude Only:</text>
        <text v-if="has_data">{{ longitude }}</text>

        <text class="text-error">{{ errorMessage }}</text>
    </view>
</template>
<script>
    import * as Location from "expo-location";
    import * as Permissions from "expo-permissions";

    export default {
        name: 'Content',
        data: function() {
            return {
                location: {},
                latitude: "",
                longitude: "",
                errorMessage: "",
                has_data:false,
            };
        },
        methods: {
            getLocation: function() {
                this.has_data = false;
                Permissions.askAsync(Permissions.LOCATION)
                    .then(status => {
                        if (!status.granted) {
                            this.errorMessage = "Permission to access location was denied";
                        } else if (status.granted) {
                            Location.getCurrentPositionAsync({}).then(location => {
                                this.has_data = true;
                                this.location = location;
                                this.latitude = location.coords.latitude
                                this.longitude = location.coords.longitude
                                this.errorMessage = "";
                            });
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    });
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
    .my-button{
        background-color: lightgray;
        border-width: 1px;
        padding: 3%;
        border-radius: 10px;
    }
</style>
