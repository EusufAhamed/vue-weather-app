<script setup>
    import axios from 'axios'
    import { reactive } from 'vue'
    import DaysWeather from './DaysWeather.vue'

    const props = defineProps({
        city: {
            type: String,
            required: true
        }
    })

    let weatherData = reactive({
        cityName: props.city,
        temperature: '',
        description: '',
        iconUrl: '',
        date: '',
        time: '',
        name: '',
        country: '',
        wind: '',
        humidity: '',
        monthNames: ['January', 'February', 'March', 'April', 'May', 'Jun', 'July', 'August', 'September', 'October', 'November', 'December']
    })

    const getWeather = async () => {
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${props.city}&units=metric&appid=e6a5d0cf02835f18f97970563b7053a8`)
        const responseData = response.data
        weatherData.temperature = Math.round(responseData.main.temp)
        weatherData.description = responseData.weather[0].description
        weatherData.name = responseData.name
        weatherData.iconUrl = `https://api.openweathermap.org/img/w/${responseData.weather[0].icon}`
        weatherData.country = responseData.sys.country
        weatherData.wind = responseData.wind.speed
        weatherData.humidity = responseData.main.humidity
        
        const d = new Date()
        weatherData.date =  weatherData.monthNames[d.getMonth()] + ' ' + d.getDate() + ', ' + d.getFullYear()
        weatherData.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds()

        // console.log(responseData)
    }

    getWeather()

    // onBeforeMount(async () => {
    //     getWeather()
    // })
</script>

<template>
    <div class="container p-0">
        <div class="d-flex">
            <div class="card main-div w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">Today</h2>
                    <p class="text-light date mb-0">{{ weatherData.date }}</p>
                    <small>{{ weatherData.time }}</small>
                    <h2 class="place"><i class="fa fa-location"> {{ weatherData.name }}, <small>{{ weatherData.country }}</small></i></h2>

                    <div class="temp">
                        <h1 class="weather-temp">{{ weatherData.temperature }}&deg;</h1>
                        <h2 class="text-light">{{ weatherData.description }} <img :src="weatherData.iconUrl"></h2>
                    </div>
                </div>
            </div>

            <div class="card card-2 w-100">
                <table class="m-4">
                    <tbody>
                        <tr>
                            <th>Sea Level</th>
                        </tr>
                        <tr>
                            <th>Humidity</th>
                            <td>{{ weatherData.humidity }}</td>
                        </tr>
                        <tr>
                            <th>Wind</th>
                            <td>{{ weatherData.wind }}</td>
                        </tr>
                    </tbody>
                </table>

                <DaysWeather :cityname="weatherData.cityName"/>

                <div id="div_Form" class="d-flex m-3 justify-content-center">
                    <form action="">
                        <input type="button" class="btn change-btn btn-primary" value="Change Location">
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
    body{
        background-color: #343d4b;
    }

    .weather-temp{
        margin: 0;
        font-weight: 700;
        font-size: 4rem;
    }

    h2.mb-1.day{
        font-size: 3rem;
        font-weight: 400;
    }

    .main-div{
        border-radius: 20px;
        color: #fff;
        background-image: url('https://plus.unsplash.com/premium_photo-1680348285644-ab9ffca7c33d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1633&q=80');
        background-size: cover;
        background-position: center;
        background-blend-mode: overlay;
        background-color: rgba(0, 0, 0, 0.5);
        background-repeat: no-repeat;
    }

    .temp{
        position: absolute;
        bottom: 0;
    }

    .main-div:hover{
        transform: scale(1.1);
        transition: transform 0.5s ease;
        z-index: 1;
    }

    .card-2{
        background-color: #212730;
        border-radius: 20px;
    }

    .card-details{
        margin-left: 19px;
    }

    .h1_Left{
        position: absolute;
        bottom: 25px;
        left: 16px;
        font-size: 3vw;
        line-height: 1.2;
    }

    .h3_Left{
        position: absolute;
        left: 16px;
        font-size: 2vw;
        line-height: 0.5;
    }

    .h3_Left small{
        font-size: 1rem;
    }

    table{
        position: relative;
        left: 15px;
        border-collapse: separate;
        border-spacing: 15px;
        width: 85%;
        text-align: left;
        max-width: 600px;
        margin: 0 auto;
    }

    th, td{
        font-size: 18px;
        color: #fff;
    }

    td{
        text-align: right;
    }

    table, tr:hover{
        color: red;
    }

    .change_btn{
        background-image: linear-gradient(to right, cyan, magenta);
    }

    .change_btn:hover{
        transform: scale(0.9);
        transition: transform 0.1s ease;
    }
</style>