# Laravel-Vue Weather App

> ### Example Laravel-Vue codebase containing simple weather-app example (uses openweather-api and algolia places for search).

----------

# Getting started

## Installation

Please check the official laravel installation guide for server requirements before you start. [Official Documentation](https://laravel.com/docs/5.4/installation#installation)


Clone the repository

    git clone https://github.com/mansoorkhan96/weatherapp.git

Switch to the repo folder

    cd weatherapp

Install all the dependencies using composer

    composer install

Install all the Vue dependencies using npm

    nmp install

Compile all the Vue dependencies using npm

    nmp run dev

Copy the example env file and make the required configuration changes in the .env file

    cp .env.example .env

Change API keys

    Algolia APP_ID (path: '/resources/js/components/WeatherApp.vue')
    Algolia API_KEY (path: '/resources/js/components/WeatherApp.vue')
    OpenWeather API_KEY (path: '.env', key: OPEN_WEATHER)

Compile all the new code changes

    nmp run watch
    or
    nmp run dev

Generate a new application key

    php artisan key:generate

Start the local development server

    php artisan serve

You can now access the server at http://localhost:8000