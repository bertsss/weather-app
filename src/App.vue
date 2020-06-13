<template>
    <div id="app" :class="weather.id && weather.main.temp > 16 ? 'warm': ''">
        <div class="loading-mask" v-if="loading">
            <div class="loader"></div>
        </div>

        <main v-else>
            <div class="search-box">
                <input 
                    type="text" 
                    class="search-bar" 
                    placeholder="Search..."
                    v-model="query"
                    @keypress="fetchWeather($event)"
                >
            </div>

            <div class="error" v-if="weather.message">{{ weather.message }}</div>
            
            <div class="weather-wrap" v-if="weather.id">
                <div class="location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
                    <div class="date">{{ dateBuilder() }}</div>
                </div>

                <div class="weather-box">
                    <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
                    <div class="weather">{{ weather.weather[0].main }}</div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    name: 'app',
    data () {
        return {
            apiKey: '3b5a57977da6e30f510fb11b7a419cf5',
            urlBase: 'https://api.openweathermap.org/data/2.5/',
            query: '',
            error: '',
            loading: false,
            weather: {}
        }
    },

    methods: {
        fetchWeather (e) {
            if (e.key === 'Enter') {
                this.loading = true
                fetch(`${this.urlBase}/weather?q=${this.query}&units=metric&APPID=${this.apiKey}`)
                .then(res => {
                    return res.json()
                })
                .then(this.setResult)
            }
        },

        setResult (result) {
            this.weather = result
            this.loading = false
        },

        dateBuilder () {
            let d = new Date()
            let months = ['January', 'February', 'March', 'April', 'June', 'July',
            'August', 'September','October', 'November', 'December']
            let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

            let day = days[d.getDay()]
            let date = d.getDate()
            let month = months[d.getMonth()]
            let year = d.getFullYear()

            return `${day} ${date} ${month} ${year}`
        }
    }
}
</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;

    body {
        font-family: 'montserrat', sans-serif;
    }

    #app {
        background-image: url('./assets/cold.jpg');
        background-size: cover;
        background-position: bottom;
        transition: 0.4s;

        &.warm {
            background-image: url('./assets/warm-bg.jpg');
        }
    }

    .loading-mask {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: rgba(0, 0, 0, 0.4);

        /deep/ .modal-content {
            background: transparent;
            box-shadow: none;
        }

        .loader {
            border-radius: 50%;
            width: 50px;
            height: 50px;
            border-top: 10px solid rgba(131, 220, 202,0.1);
            border-right: 10px solid rgba(131, 220, 202,0.3);
            border-bottom: 10px solid rgba(131, 220, 202,0.5);
            border-left: 10px solid rgba(131, 220, 202,0.8);;
            animation: animate-rotate infinite linear 1s;
        }

        @keyframes animate-rotate{

            0% {
                transform: rotate(0deg);
            }
            50%{
                transform: rotate(180deg);
                opacity: .35;
            }
            100%{
                transform: rotate(360deg);
            }   
        }
    }

    main {
        min-height: 100vh;
        padding: 25px;
        background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));

        .error {
            text-align: center;
            color: #fff;
            text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
            font-size: 24px;
            text-transform: capitalize;
        }

        .search-box {
            width: 100%;
            margin-bottom: 25px;

            .search-bar {
                display: block;
                width: 100%;
                padding: 15px;

                color: #313131;
                font-size: 20px;

                appearance: none;
                border: none;
                outline: none;
                background: none;

                box-shadow: 0px 0px 16px rgba(0,0,0,0.75);
                background-color: rgba(255,255,255,0.5);
                border-radius: 0px 16px 0px 16px;
                transition: 0.4s;

                &:focus {
                    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
                    background-color: rgba(255,255,255,0.75);
                    border-radius: 16px 0px 16px 0px;
                }
            }
        }

        .weather-wrap {
            text-align: center;
            .location-box {
                color: #fff;

                .location {
                    font-size: 32px;
                    font-weight: 500;
                    text-shadow: 1px 3px rgba(0,0,0,0.25);
                }

                .date {
                    font-size: 20px;
                    font-weight: 300;
                    font-style: italic;
                }
            }
            
            .weather-box {
                color: #fff;

                .temp {
                    font-size: 102px;
                    font-weight: 900;
                    display: inline-block;
                    padding: 10px 25px;
                    text-shadow: 1px 3px rgba(0,0,0,0.25);
                    background-color: rgba(255,255,255,0.25);
                    border-radius: 16px;
                    margin: 30px 0px;
                    box-shadow: 3px 6px rgba(0,0,0,0.25);
                }

                .weather {
                    font-size: 48px;
                    font-weight: 700;
                    font-style: italic;
                    text-shadow: 3px 6px rgba(0,0,0,0.25);
                }
            }
        }
    }
}
</style>
