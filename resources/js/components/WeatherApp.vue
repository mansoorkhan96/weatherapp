<template>
    <div class="text-white mb-8">
        <div class="places-input text-gray-800">
            <input type="search" id="address" class="form-control" placeholder="Where are we going?" />

            <p>Selected: <strong id="address-value">none</strong></p>
        </div>
        <div class="weather-container font-sans w-128 max-w-lg rounded-lg overflow-hidden bg-teal-700 shadow-lg mt-4">
            <div class="current-weather flex items-center justify-between px-6 py-8">
                <div class="flex items-center">
                    <div>
                        <div class="text-6xl font-semibold">{{ currentTemperature.actual }}°C</div>
                        <div>Feels like {{ currentTemperature.feels }}°C</div>
                    </div>
                    <div class="mx-5">
                        <div class="font-semibold">{{ currentTemperature.summary.main }} <small>({{ currentTemperature.summary.description }})</small></div>
                        <div v-text="location.name"></div>
                    </div>
                </div>
                <div><img :src="currentTemperature.icon" alt=""></div>
            </div> <!-- end current-weather -->

            <div class="future-weather text-sm bg-teal-600 px-6 py-8 overflow-hidden">

                <div 
                v-for="(day, index) in daily" 
                :key="day.dt"
                :class="{ 'mt-4': index > 0 }"
                class="flex items-center">
                    <div class="w-1/6 text-lg text-gray-200">{{ toDayOfWeek(day.dt) }}</div>
                    <div class="w-4/6 px-4 flex items-center">
                        <div><img :src="weatherIcon(day.weather[0].icon)" alt=""></div>
                        <div class="ml-3">{{ day.weather[0].main }} <small>({{ day.weather[0].description }})</small></div>
                    </div>
                    <div class="w-1/6 text-right">
                        <div>{{ Math.round(day.temp.max) }}</div>
                        <div>{{ Math.round(day.temp.min) }}</div>
                    </div>
                </div>

            </div> <!-- end future-container -->
            
        </div> <!-- end weather-container -->
    </div>
</template>

<script>
export default {
    name: 'WeatherApp',
    mounted() {
        this.fetchData()

        var placesAutocomplete = places({
            appId: 'app_id',
            apiKey: 'api_key',
            container: document.querySelector('#address')
        }).configure({
            type: 'city',
            aroundLatLngViaIP: false,
        });

        var $address = document.querySelector('#address-value')
        placesAutocomplete.on('change', (e) => {
            $address.textContent = e.suggestion.value

            this.location.name = `${e.suggestion.name}, ${e.suggestion.country}`
            this.location.lat = e.suggestion.latlng.lat;
            this.location.lon = e.suggestion.latlng.lng;
        });

        placesAutocomplete.on('clear', function() {
            $address.textContent = 'none';
        });
    },

    watch: {
        location: {
            handler(newValue, oldValue){
                this.fetchData()
            },
            deep: true
        }
    },

    data() {
        return {
            currentTemperature: {
                actual: '30',
                feels: '33',
                summary: {
                    main: 'Rainy',
                    description: ''
                },
                icon: 'http://openweathermap.org/img/wn/10d@2x.png',
            },

            daily: [],

            location: {
                name: 'Sehwan, Pakistan',
                lat: 26.4122,
                lon: 67.8629
            }
        }
    },

    methods: {
        fetchData() {
            fetch(`/api/weather?lat=${this.location.lat}&lon=${this.location.lon}`)
            .then(response => response.json())
            .then(data => {
                this.currentTemperature.icon = `http://openweathermap.org/img/wn/${data.current.weather[0].icon}@2x.png`;
                this.currentTemperature.summary.main = data.current.weather[0].main;
                this.currentTemperature.summary.description = data.current.weather[0].description;

                this.currentTemperature.actual = Math.round(data.current.temp);
                this.currentTemperature.feels = Math.round(data.current.feels_like);

                this.daily = data.daily;
            })
        },

        weatherIcon(icon) {
            return `http://openweathermap.org/img/wn/${icon}.png`;
        },

        toDayOfWeek(timestamp) {
            const newDate = new Date(timestamp*1000);
            const days = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
            return days[newDate.getDay()]
        }

    }
}
</script>
