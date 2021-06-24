<template>
  <div>
    <div class="main-container">
      <div
        class="animation-bg"
        :class="[
          { clear: this.clear },
          { cloud: this.cloud },
          { rain: this.rain },
          { thunder: this.thunder },
          { snow: this.snow },
          { mist: this.mist }
        ]"
      ></div>

      <header class="header">
        <h1 class="header__title">
          Weather App
        </h1>
      </header>

      <main class="main-contents">
        <div class="main-contents__input-area">
          <form class="main-contents__form">
            <fieldset class="main-contents__fieldset">
              <label class="main-contents__label">都市名で検索</label>
              <p
                class="main-contents__validation-message"
                v-show="cityValidMessage1"
              >
                入力内容が正しくありません。
              </p>
              <p
                class="main-contents__validation-message"
                v-show="cityValidMessage2"
              >
                都市名を入力してください。
              </p>
              <input
                class="main-contents__input main-contents__input--search-city"
                v-model="searchCity"
                placeholder="tokyo"
                type="text"
              />
              <button
                class="main-contents__btn"
                @click.prevent="
                  getSearchCityWeather(),
                    getSearchCityWeekWeather(),
                    cityValidation(searchCity)
                "
              >
                検索
              </button>
            </fieldset>
          </form>

          <form class="main-contents__form">
            <fieldset class="main-contents__fieldset">
              <label class="main-contents__label">郵便番号で検索</label>
              <p
                class="main-contents__validation-message"
                v-show="zipValidMessage1"
              >
                入力内容が正しくありません。
              </p>
              <p
                class="main-contents__validation-message"
                v-show="zipValidMessage2"
              >
                郵便番号を入力してください。
              </p>

              <input
                class="main-contents__input main-contents__input--search-zip"
                v-model="searchZip"
                placeholder="000-0000"
                type="tel"
              />
              <button
                class="main-contents__btn"
                @click.prevent="
                  getZipCodeWeather(),
                    getZipCodeWeekWeather(),
                    zipValidation(searchZip)
                "
              >
                検索
              </button>
            </fieldset>
          </form>
          <form class="main-contents__form">
            <fieldset class="main-contents__fieldset">
              <label class="main-contents__label">現在地の天気を取得</label>
              <button
                class="main-contents__btn"
                @click.prevent="getCurrentWeather(), getCurrentWeekWeather()"
              >
                取得
              </button>
            </fieldset>
          </form>
        </div>

        <div class="main-contents__weather-state-message"></div>

        <div class="main-contents__output-area">
          <table class="main-contents__table">
            <thead class="main-contents__thead">
              <tr
                class="
              main-contents__tr--date
              main-contents__tr--date--first
              "
              >
                <th
                  class="
                main-contents__th--date
                "
                >
                  {{ today }}
                </th>
              </tr>
              <tr
                class="
              main-contents__tr--sort
              main-contents__tr--sort--first
              "
              >
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  場所
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  天気
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  現在の気温
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  最高気温
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  最低気温
                </th>
              </tr>
            </thead>
            <tbody class="main-contents__tbody">
              <tr
                class="
              main-contents__tr--sort
              "
              >
                <td
                  class="
                main-contents__td
                weather-spot
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-icon
                "
                >
                  <img
                    class="
                  main-contents__td-icon
                  weather-icon__img
                  "
                    src=""
                    alt=""
                  />
                </td>
                <td
                  class="
                main-contents__td
                weather-current-temp
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-max-temp
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-min-temp
                "
                ></td>
              </tr>
            </tbody>
          </table>

          <table
            class="
            main-contents__table
            "
          >
            <thead class="main-contents__thead">
              <tr
                class="
              main-contents__tr--date
              main-contents__tr--date--second
              "
              >
                <th
                  class="
                main-contents__th--date
                "
                >
                  週間天気
                </th>
              </tr>
              <tr
                class="
              main-contents__tr--sort
              main-contents__tr--sort--second
              "
              >
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  場所
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  天気
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  最高気温
                </th>
                <th
                  class="
                  main-contents__th--sort
                  "
                >
                  最低気温
                </th>
              </tr>
            </thead>
            <tbody class="main-contents__tbody">
              <tr
                class="
              main-contents__tr--date
              main-contents__tr--date--second
              "
              >
                <th
                  class="
                main-contents__th--date
                "
                >
                  {{ tomorrow }}
                </th>
              </tr>
              <tr
                class="
              main-contents__tr--sort
              main-contents__tr--sort--second
              "
              >
                <td
                  class="
                main-contents__td
                weather-week-spot--1
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week--1
                weather-icon
                "
                >
                  <img
                    class="
                  main-contents__td-icon
                  weather-icon__img--1
                  "
                    src=""
                    alt=""
                  />
                </td>
                <td
                  class="
                main-contents__td
                weather-week-max-temp--1
                w-1/5
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week-min-temp--1
                "
                ></td>
              </tr>
              <tr
                class="
              main-contents__tr--date
              main-contents__tr--date--second
              "
              >
                <th
                  class="
                main-contents__th--date
                "
                >
                  {{ afterTomorrow }}
                </th>
              </tr>
              <tr
                class="
                main-contents__tr--sort
                main-contents__tr--sort--second
                "
              >
                <td
                  class="
                main-contents__td
                weather-week-spot--2
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week--2
                weather-icon
                "
                >
                  <img
                    class="
                  main-contents__td-icon
                  weather-icon__img--2
                  "
                    src=""
                    alt=""
                  />
                </td>
                <td
                  class="
                main-contents__td
                weather-week-max-temp--2
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week-min-temp--2
                "
                ></td>
              </tr>
              <tr
                class="
              main-contents__tr--date
              main-contents__tr--date--second
              "
              >
                <th
                  class="
                main-contents__th--date
                "
                >
                  {{ twoDaysAfterTomorrow }}
                </th>
              </tr>
              <tr
                class="
                main-contents__tr--sort
                "
              >
                <td
                  class="
                main-contents__td
                weather-week-spot--3
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week--3
                weather-icon
                "
                >
                  <img
                    class="
                  main-contents__td-icon
                  weather-icon__img--3
                  "
                    src=""
                    alt=""
                  />
                </td>
                <td
                  class="
                main-contents__td
                weather-week-max-temp--3
                "
                ></td>
                <td
                  class="
                main-contents__td
                weather-week-min-temp--3
                "
                ></td>
              </tr>
            </tbody>
          </table>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      searchCity: "",
      searchZip: "",
      cityValidMessage1: false,
      cityValidMessage2: false,
      zipValidMessage1: false,
      zipValidMessage2: false,
      clear: false,
      cloud: false,
      rain: false,
      thunder: false,
      snow: false,
      mist: false
    };
  },
  computed: {
    //今日の日付
    today() {
      let day = new Date();
      let month = day.getMonth() + 1;
      let date = day.getDate();
      return `${month}月${date}日`;
    },
    //明日の日付
    tomorrow() {
      let day = new Date();
      let month = day.getMonth() + 1;
      let date = day.getDate() + 1;
      return `${month}月${date}日`;
    },
    //明後日の日付
    afterTomorrow() {
      let day = new Date();
      let month = day.getMonth() + 1;
      let date = day.getDate() + 2;
      return `${month}月${date}日`;
    },
    //明々後日の日付
    twoDaysAfterTomorrow() {
      let day = new Date();
      let month = day.getMonth() + 1;
      let date = day.getDate() + 3;
      return `${month}月${date}日`;
    }
  },
  methods: {
    //都市名検索による今日の天気取得&アイコン変更処理気
    getSearchCityWeather() {
      let searchCity = document.querySelector(
        ".main-contents__input--search-city"
      );
      let searchZip = document.querySelector(
        ".main-contents__input--search-zip"
      );
      let spot = document.querySelector(".weather-spot");
      let temp = document.querySelector(".weather-current-temp");
      let maxTemp = document.querySelector(".weather-max-temp");
      let minTemp = document.querySelector(".weather-min-temp");
      let weatherList = [];
      let weatherIconImg = document.querySelector(".weather-icon__img");
      let url = "http://api.openweathermap.org/data/2.5/weather?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";
      this.$axios
        .get(
          url +
            "q=" +
            searchCity.value +
            ",jp&units=metric&lang=ja&appid=" +
            apiKey
        )
        .then(val => {
          weatherList.push(val);
          console.log("Run GetSearchCityWeatherSuccess");
          //テキスト挿入
          spot.innerHTML = weatherList[0].data.name;
          temp.innerHTML = Math.round(weatherList[0].data.main.temp) + "℃";
          maxTemp.innerHTML =
            Math.round(weatherList[0].data.main.temp_max) + "℃";
          minTemp.innerHTML =
            Math.round(weatherList[0].data.main.temp_min) + "℃";
          //input初期化
          searchCity.value = "";
          searchZip.value = "";
          //アイコン条件分岐
          // Clear [01]
          if (weatherList[0].data.weather[0].id === 800) {
            weatherIconImg.src = iconUrl + "01d@2x.png";
            this.createClear();
            this.createTextClear();
          }
          // Clouds [02]
          else if (weatherList[0].data.weather[0].id === 801) {
            weatherIconImg.src = iconUrl + "02d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Clouds [03]
          else if (weatherList[0].data.weather[0].id === 802) {
            weatherIconImg.src = iconUrl + "03d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Clouds [04]
          else if (
            weatherList[0].data.weather[0].id === 803 ||
            weatherList[0].data.weather[0].id === 804
          ) {
            weatherIconImg.src = iconUrl + "04d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Rain [09]
          else if (
            weatherList[0].data.weather[0].id === 300 ||
            weatherList[0].data.weather[0].id === 301 ||
            weatherList[0].data.weather[0].id === 302 ||
            weatherList[0].data.weather[0].id === 310 ||
            weatherList[0].data.weather[0].id === 311 ||
            weatherList[0].data.weather[0].id === 312 ||
            weatherList[0].data.weather[0].id === 313 ||
            weatherList[0].data.weather[0].id === 314 ||
            weatherList[0].data.weather[0].id === 321 ||
            weatherList[0].data.weather[0].id === 520 ||
            weatherList[0].data.weather[0].id === 521 ||
            weatherList[0].data.weather[0].id === 522 ||
            weatherList[0].data.weather[0].id === 531
          ) {
            weatherIconImg.src = iconUrl + "09d@2x.png";
            this.createRain();
            this.createTextRain();
          }
          // Rain [10]
          else if (
            weatherList[0].data.weather[0].id === 500 ||
            weatherList[0].data.weather[0].id === 501 ||
            weatherList[0].data.weather[0].id === 502 ||
            weatherList[0].data.weather[0].id === 503 ||
            weatherList[0].data.weather[0].id === 504
          ) {
            weatherIconImg.src = iconUrl + "10d@2x.png";
            this.createRain();
            this.createTextRain();
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.weather[0].id === 200 ||
            weatherList[0].data.weather[0].id === 201 ||
            weatherList[0].data.weather[0].id === 202 ||
            weatherList[0].data.weather[0].id === 210 ||
            weatherList[0].data.weather[0].id === 211 ||
            weatherList[0].data.weather[0].id === 212 ||
            weatherList[0].data.weather[0].id === 221 ||
            weatherList[0].data.weather[0].id === 230 ||
            weatherList[0].data.weather[0].id === 231 ||
            weatherList[0].data.weather[0].id === 232
          ) {
            weatherIconImg.src = iconUrl + "11d@2x.png";
            this.createThunder();
            this.createTextThunder();
          }
          // Snow [13]
          else if (
            weatherList[0].data.weather[0].id === 511 ||
            weatherList[0].data.weather[0].id === 600 ||
            weatherList[0].data.weather[0].id === 601 ||
            weatherList[0].data.weather[0].id === 602 ||
            weatherList[0].data.weather[0].id === 611 ||
            weatherList[0].data.weather[0].id === 612 ||
            weatherList[0].data.weather[0].id === 613 ||
            weatherList[0].data.weather[0].id === 615 ||
            weatherList[0].data.weather[0].id === 616 ||
            weatherList[0].data.weather[0].id === 620 ||
            weatherList[0].data.weather[0].id === 621 ||
            weatherList[0].data.weather[0].id === 622
          ) {
            weatherIconImg.src = iconUrl + "13d@2x.png";
            this.createSnow();
            this.createTextSnow();
          }
          // Mist [50]
          else if (
            weatherList[0].data.weather[0].id === 701 ||
            weatherList[0].data.weather[0].id === 711 ||
            weatherList[0].data.weather[0].id === 721 ||
            weatherList[0].data.weather[0].id === 731 ||
            weatherList[0].data.weather[0].id === 741 ||
            weatherList[0].data.weather[0].id === 751 ||
            weatherList[0].data.weather[0].id === 761 ||
            weatherList[0].data.weather[0].id === 762 ||
            weatherList[0].data.weather[0].id === 771 ||
            weatherList[0].data.weather[0].id === 781
          ) {
            weatherIconImg.src = iconUrl + "50d@2x.png";
            this.createMist();
            this.createTextMist();
          }
        })
        .catch(e => {
          console.error("Run GetSearchCityWeatherError..." + e);
        });
    },

    //都市名検索による週間天気取得&アイコン変更処理
    getSearchCityWeekWeather() {
      let searchCity = document.querySelector(
        ".main-contents__input--search-city"
      );
      let searchZip = document.querySelector(
        ".main-contents__input--search-zip"
      );
      let container = document.querySelector(".main-container");
      //明日
      let weekSpot1 = document.querySelector(".weather-week-spot--1");
      let weekMaxTemp1 = document.querySelector(".weather-week-max-temp--1");
      let weekMinTemp1 = document.querySelector(".weather-week-min-temp--1");
      //明後日
      let weekSpot2 = document.querySelector(".weather-week-spot--2");
      let weekMaxTemp2 = document.querySelector(".weather-week-max-temp--2");
      let weekMinTemp2 = document.querySelector(".weather-week-min-temp--2");
      //明々後日
      let weekSpot3 = document.querySelector(".weather-week-spot--3");
      let weekMaxTemp3 = document.querySelector(".weather-week-max-temp--3");
      let weekMinTemp3 = document.querySelector(".weather-week-min-temp--3");
      let weatherList = [];
      let weatherIconImg1 = document.querySelector(".weather-icon__img--1");
      let weatherIconImg2 = document.querySelector(".weather-icon__img--2");
      let weatherIconImg3 = document.querySelector(".weather-icon__img--3");
      let url = "http://api.openweathermap.org/data/2.5/forecast?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";

      this.$axios
        .get(
          url +
            "q=" +
            searchCity.value +
            ",jp&units=metric&lang=ja&cnt=3&appid=" +
            apiKey
        )
        .then(val => {
          weatherList.push(val);
          container.style.background = "none";
          console.log("Run GetSearchCityWeekWeatherSuccess");
          //テキスト挿入
          //明日
          weekSpot1.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp1.innerHTML =
            Math.round(weatherList[0].data.list[0].main.temp_max) + "℃";
          weekMinTemp1.innerHTML =
            Math.round(weatherList[0].data.list[0].main.temp_min) + "℃";
          //明後日
          weekSpot2.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp2.innerHTML =
            Math.round(weatherList[0].data.list[1].main.temp_max) + "℃";
          weekMinTemp2.innerHTML =
            Math.round(weatherList[0].data.list[1].main.temp_min) + "℃";
          //明々後日
          weekSpot3.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp3.innerHTML =
            Math.round(weatherList[0].data.list[2].main.temp_max) + "℃";
          weekMinTemp3.innerHTML =
            Math.round(weatherList[0].data.list[2].main.temp_min) + "℃";
          //input初期化
          searchCity.value = "";
          searchZip.value = "";
          //アイコン条件分岐 (明日)
          // Clear [01]
          if (weatherList[0].data.list[0].weather[0].id === 800) {
            weatherIconImg1.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[0].weather[0].id === 801) {
            weatherIconImg1.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[0].weather[0].id === 802) {
            weatherIconImg1.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[0].weather[0].id === 803 ||
            weatherList[0].data.list[0].weather[0].id === 804
          ) {
            weatherIconImg1.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[0].weather[0].id === 300 ||
            weatherList[0].data.list[0].weather[0].id === 301 ||
            weatherList[0].data.list[0].weather[0].id === 302 ||
            weatherList[0].data.list[0].weather[0].id === 310 ||
            weatherList[0].data.list[0].weather[0].id === 311 ||
            weatherList[0].data.list[0].weather[0].id === 312 ||
            weatherList[0].data.list[0].weather[0].id === 313 ||
            weatherList[0].data.list[0].weather[0].id === 314 ||
            weatherList[0].data.list[0].weather[0].id === 321 ||
            weatherList[0].data.list[0].weather[0].id === 520 ||
            weatherList[0].data.list[0].weather[0].id === 521 ||
            weatherList[0].data.list[0].weather[0].id === 522 ||
            weatherList[0].data.list[0].weather[0].id === 531
          ) {
            weatherIconImg1.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[0].weather[0].id === 500 ||
            weatherList[0].data.list[0].weather[0].id === 501 ||
            weatherList[0].data.list[0].weather[0].id === 502 ||
            weatherList[0].data.list[0].weather[0].id === 503 ||
            weatherList[0].data.list[0].weather[0].id === 504
          ) {
            weatherIconImg1.src = iconUrl + "10d@2x.png";
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.list[0].weather[0].id === 200 ||
            weatherList[0].data.list[0].weather[0].id === 201 ||
            weatherList[0].data.list[0].weather[0].id === 202 ||
            weatherList[0].data.list[0].weather[0].id === 210 ||
            weatherList[0].data.list[0].weather[0].id === 211 ||
            weatherList[0].data.list[0].weather[0].id === 212 ||
            weatherList[0].data.list[0].weather[0].id === 221 ||
            weatherList[0].data.list[0].weather[0].id === 230 ||
            weatherList[0].data.list[0].weather[0].id === 231 ||
            weatherList[0].data.list[0].weather[0].id === 232
          ) {
            weatherIconImg1.src = iconUrl + "11d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[0].weather[0].id === 511 ||
            weatherList[0].data.list[0].weather[0].id === 600 ||
            weatherList[0].data.list[0].weather[0].id === 601 ||
            weatherList[0].data.list[0].weather[0].id === 602 ||
            weatherList[0].data.list[0].weather[0].id === 611 ||
            weatherList[0].data.list[0].weather[0].id === 612 ||
            weatherList[0].data.list[0].weather[0].id === 613 ||
            weatherList[0].data.list[0].weather[0].id === 615 ||
            weatherList[0].data.list[0].weather[0].id === 616 ||
            weatherList[0].data.list[0].weather[0].id === 620 ||
            weatherList[0].data.list[0].weather[0].id === 621 ||
            weatherList[0].data.list[0].weather[0].id === 622
          ) {
            weatherIconImg1.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[0].weather[0].id === 701 ||
            weatherList[0].data.list[0].weather[0].id === 711 ||
            weatherList[0].data.list[0].weather[0].id === 721 ||
            weatherList[0].data.list[0].weather[0].id === 731 ||
            weatherList[0].data.list[0].weather[0].id === 741 ||
            weatherList[0].data.list[0].weather[0].id === 751 ||
            weatherList[0].data.list[0].weather[0].id === 761 ||
            weatherList[0].data.list[0].weather[0].id === 762 ||
            weatherList[0].data.list[0].weather[0].id === 771 ||
            weatherList[0].data.list[0].weather[0].id === 781
          ) {
            weatherIconImg1.src = iconUrl + "50d@2x.png";
          }
          //アイコン条件分岐 (明後日)
          // Clear [01]
          if (weatherList[0].data.list[1].weather[0].id === 800) {
            weatherIconImg2.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[1].weather[0].id === 801) {
            weatherIconImg2.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[1].weather[0].id === 802) {
            weatherIconImg2.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[1].weather[0].id === 803 ||
            weatherList[0].data.list[1].weather[0].id === 804
          ) {
            weatherIconImg2.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[1].weather[0].id === 300 ||
            weatherList[0].data.list[1].weather[0].id === 301 ||
            weatherList[0].data.list[1].weather[0].id === 302 ||
            weatherList[0].data.list[1].weather[0].id === 310 ||
            weatherList[0].data.list[1].weather[0].id === 311 ||
            weatherList[0].data.list[1].weather[0].id === 312 ||
            weatherList[0].data.list[1].weather[0].id === 313 ||
            weatherList[0].data.list[1].weather[0].id === 314 ||
            weatherList[0].data.list[1].weather[0].id === 321 ||
            weatherList[0].data.list[1].weather[0].id === 520 ||
            weatherList[0].data.list[1].weather[0].id === 521 ||
            weatherList[0].data.list[1].weather[0].id === 522 ||
            weatherList[0].data.list[1].weather[0].id === 531
          ) {
            weatherIconImg2.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[1].weather[0].id === 500 ||
            weatherList[0].data.list[1].weather[0].id === 501 ||
            weatherList[0].data.list[1].weather[0].id === 502 ||
            weatherList[0].data.list[1].weather[0].id === 503 ||
            weatherList[0].data.list[1].weather[0].id === 504
          ) {
            weatherIconImg2.src = iconUrl + "10d@2x.png";
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.list[1].weather[0].id === 200 ||
            weatherList[0].data.list[1].weather[0].id === 201 ||
            weatherList[0].data.list[1].weather[0].id === 202 ||
            weatherList[0].data.list[1].weather[0].id === 210 ||
            weatherList[0].data.list[1].weather[0].id === 211 ||
            weatherList[0].data.list[1].weather[0].id === 212 ||
            weatherList[0].data.list[1].weather[0].id === 221 ||
            weatherList[0].data.list[1].weather[0].id === 230 ||
            weatherList[0].data.list[1].weather[0].id === 231 ||
            weatherList[0].data.list[1].weather[0].id === 232
          ) {
            weatherIconImg2.src = iconUrl + "11d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[1].weather[0].id === 511 ||
            weatherList[0].data.list[1].weather[0].id === 600 ||
            weatherList[0].data.list[1].weather[0].id === 601 ||
            weatherList[0].data.list[1].weather[0].id === 602 ||
            weatherList[0].data.list[1].weather[0].id === 611 ||
            weatherList[0].data.list[1].weather[0].id === 612 ||
            weatherList[0].data.list[1].weather[0].id === 613 ||
            weatherList[0].data.list[1].weather[0].id === 615 ||
            weatherList[0].data.list[1].weather[0].id === 616 ||
            weatherList[0].data.list[1].weather[0].id === 620 ||
            weatherList[0].data.list[1].weather[0].id === 621 ||
            weatherList[0].data.list[1].weather[0].id === 622
          ) {
            weatherIconImg2.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[1].weather[0].id === 701 ||
            weatherList[0].data.list[1].weather[0].id === 711 ||
            weatherList[0].data.list[1].weather[0].id === 721 ||
            weatherList[0].data.list[1].weather[0].id === 731 ||
            weatherList[0].data.list[1].weather[0].id === 741 ||
            weatherList[0].data.list[1].weather[0].id === 751 ||
            weatherList[0].data.list[1].weather[0].id === 761 ||
            weatherList[0].data.list[1].weather[0].id === 762 ||
            weatherList[0].data.list[1].weather[0].id === 771 ||
            weatherList[0].data.list[1].weather[0].id === 781
          ) {
            weatherIconImg2.src = iconUrl + "50d@2x.png";
          }
          //アイコン条件分岐 (明々後日)
          // Clear [01]
          if (weatherList[0].data.list[2].weather[0].id === 800) {
            weatherIconImg3.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[2].weather[0].id === 801) {
            weatherIconImg3.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[2].weather[0].id === 802) {
            weatherIconImg3.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[2].weather[0].id === 803 ||
            weatherList[0].data.list[2].weather[0].id === 804
          ) {
            weatherIconImg3.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[2].weather[0].id === 300 ||
            weatherList[0].data.list[2].weather[0].id === 301 ||
            weatherList[0].data.list[2].weather[0].id === 302 ||
            weatherList[0].data.list[2].weather[0].id === 310 ||
            weatherList[0].data.list[2].weather[0].id === 311 ||
            weatherList[0].data.list[2].weather[0].id === 312 ||
            weatherList[0].data.list[2].weather[0].id === 313 ||
            weatherList[0].data.list[2].weather[0].id === 314 ||
            weatherList[0].data.list[2].weather[0].id === 321 ||
            weatherList[0].data.list[2].weather[0].id === 520 ||
            weatherList[0].data.list[2].weather[0].id === 521 ||
            weatherList[0].data.list[2].weather[0].id === 522 ||
            weatherList[0].data.list[2].weather[0].id === 531
          ) {
            weatherIconImg3.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[2].weather[0].id === 500 ||
            weatherList[0].data.list[2].weather[0].id === 501 ||
            weatherList[0].data.list[2].weather[0].id === 502 ||
            weatherList[0].data.list[2].weather[0].id === 503 ||
            weatherList[0].data.list[2].weather[0].id === 504
          ) {
            weatherIconImg3.src = iconUrl + "10d@2x.png";
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.list[2].weather[0].id === 200 ||
            weatherList[0].data.list[2].weather[0].id === 201 ||
            weatherList[0].data.list[2].weather[0].id === 202 ||
            weatherList[0].data.list[2].weather[0].id === 210 ||
            weatherList[0].data.list[2].weather[0].id === 211 ||
            weatherList[0].data.list[2].weather[0].id === 212 ||
            weatherList[0].data.list[2].weather[0].id === 221 ||
            weatherList[0].data.list[2].weather[0].id === 230 ||
            weatherList[0].data.list[2].weather[0].id === 231 ||
            weatherList[0].data.list[2].weather[0].id === 232
          ) {
            weatherIconImg3.src = iconUrl + "11d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[2].weather[0].id === 511 ||
            weatherList[0].data.list[2].weather[0].id === 600 ||
            weatherList[0].data.list[2].weather[0].id === 601 ||
            weatherList[0].data.list[2].weather[0].id === 602 ||
            weatherList[0].data.list[2].weather[0].id === 611 ||
            weatherList[0].data.list[2].weather[0].id === 612 ||
            weatherList[0].data.list[2].weather[0].id === 613 ||
            weatherList[0].data.list[2].weather[0].id === 615 ||
            weatherList[0].data.list[2].weather[0].id === 616 ||
            weatherList[0].data.list[2].weather[0].id === 620 ||
            weatherList[0].data.list[2].weather[0].id === 621 ||
            weatherList[0].data.list[2].weather[0].id === 622
          ) {
            weatherIconImg3.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[2].weather[0].id === 701 ||
            weatherList[0].data.list[2].weather[0].id === 711 ||
            weatherList[0].data.list[2].weather[0].id === 721 ||
            weatherList[0].data.list[2].weather[0].id === 731 ||
            weatherList[0].data.list[2].weather[0].id === 741 ||
            weatherList[0].data.list[2].weather[0].id === 751 ||
            weatherList[0].data.list[2].weather[0].id === 761 ||
            weatherList[0].data.list[2].weather[0].id === 762 ||
            weatherList[0].data.list[2].weather[0].id === 771 ||
            weatherList[0].data.list[2].weather[0].id === 781
          ) {
            weatherIconImg3.src = iconUrl + "50d@2x.png";
          }
        })
        .catch(e => {
          console.error("Run GetSearchCityWeekWeatherError..." + e);
        });
    },

    //郵便番号検索による今日の天気取得&アイコン変更処理
    getZipCodeWeather() {
      let searchCity = document.querySelector(
        ".main-contents__input--search-city"
      );
      let searchZip = document.querySelector(
        ".main-contents__input--search-zip"
      );
      let spot = document.querySelector(".weather-spot");
      let temp = document.querySelector(".weather-current-temp");
      let maxTemp = document.querySelector(".weather-max-temp");
      let minTemp = document.querySelector(".weather-min-temp");
      let weatherList = [];
      let weatherIconImg = document.querySelector(".weather-icon__img");
      let url = "http://api.openweathermap.org/data/2.5/weather?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";
      let container = document.querySelector(".main-container");
      //バリデーション
      let regexp = new RegExp(/^\d{3}-\d{4}$/);
      if (searchZip.value == "" && regexp.test(searchZip.value)) {
        this.zipValidMessage = true;
        console.log("run zipValidation");
        return;
      } else if (!searchZip.value == "") {
        this.zipValidMessage = false;
      }
      this.$axios
        .get(
          url +
            "zip=" +
            searchZip.value +
            ",jp&units=metric&lang=ja&appid=" +
            apiKey
        )
        .then(val => {
          weatherList.push(val);
          console.log(weatherList);
          console.log("Run GetZipCodeWeatherSuccess");
          //テキスト挿入
          spot.innerHTML = weatherList[0].data.name;
          temp.innerHTML = Math.round(weatherList[0].data.main.temp) + "℃";
          maxTemp.innerHTML =
            Math.round(weatherList[0].data.main.temp_max) + "℃";
          minTemp.innerHTML =
            Math.round(weatherList[0].data.main.temp_min) + "℃";
          //input初期化
          searchCity.value = "";
          searchZip.value = "";
          container.classList.remove("bg-gray-200");
          //アイコン条件分岐
          // Clear [01]
          if (weatherList[0].data.weather[0].id === 800) {
            weatherIconImg.src = iconUrl + "01d@2x.png";
            this.createClear();
            this.createTextClear();
          }
          // Clouds [02]
          else if (weatherList[0].data.weather[0].id === 801) {
            weatherIconImg.src = iconUrl + "02d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Clouds [03]
          else if (weatherList[0].data.weather[0].id === 802) {
            weatherIconImg.src = iconUrl + "03d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Clouds [04]
          else if (
            weatherList[0].data.weather[0].id === 803 ||
            weatherList[0].data.weather[0].id === 804
          ) {
            weatherIconImg.src = iconUrl + "04d@2x.png";
            this.createCloud();
            this.createTextCloud();
          }
          // Rain [09]
          else if (
            weatherList[0].data.weather[0].id === 300 ||
            weatherList[0].data.weather[0].id === 301 ||
            weatherList[0].data.weather[0].id === 302 ||
            weatherList[0].data.weather[0].id === 310 ||
            weatherList[0].data.weather[0].id === 311 ||
            weatherList[0].data.weather[0].id === 312 ||
            weatherList[0].data.weather[0].id === 313 ||
            weatherList[0].data.weather[0].id === 314 ||
            weatherList[0].data.weather[0].id === 321 ||
            weatherList[0].data.weather[0].id === 520 ||
            weatherList[0].data.weather[0].id === 521 ||
            weatherList[0].data.weather[0].id === 522 ||
            weatherList[0].data.weather[0].id === 531
          ) {
            weatherIconImg.src = iconUrl + "09d@2x.png";
            this.createRain();
            this.createTextCloud();
          }
          // Rain [10]
          else if (
            weatherList[0].data.weather[0].id === 500 ||
            weatherList[0].data.weather[0].id === 501 ||
            weatherList[0].data.weather[0].id === 502 ||
            weatherList[0].data.weather[0].id === 503 ||
            weatherList[0].data.weather[0].id === 504
          ) {
            weatherIconImg.src = iconUrl + "10d@2x.png";
            this.createRain();
            this.createTextRain();
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.weather[0].id === 200 ||
            weatherList[0].data.weather[0].id === 201 ||
            weatherList[0].data.weather[0].id === 202 ||
            weatherList[0].data.weather[0].id === 210 ||
            weatherList[0].data.weather[0].id === 211 ||
            weatherList[0].data.weather[0].id === 212 ||
            weatherList[0].data.weather[0].id === 221 ||
            weatherList[0].data.weather[0].id === 230 ||
            weatherList[0].data.weather[0].id === 231 ||
            weatherList[0].data.weather[0].id === 232
          ) {
            weatherIconImg.src = iconUrl + "11d@2x.png";
            this.createThunder();
            this.createTextThunder();
          }
          // Snow [13]
          else if (
            weatherList[0].data.weather[0].id === 511 ||
            weatherList[0].data.weather[0].id === 600 ||
            weatherList[0].data.weather[0].id === 601 ||
            weatherList[0].data.weather[0].id === 602 ||
            weatherList[0].data.weather[0].id === 611 ||
            weatherList[0].data.weather[0].id === 612 ||
            weatherList[0].data.weather[0].id === 613 ||
            weatherList[0].data.weather[0].id === 615 ||
            weatherList[0].data.weather[0].id === 616 ||
            weatherList[0].data.weather[0].id === 620 ||
            weatherList[0].data.weather[0].id === 621 ||
            weatherList[0].data.weather[0].id === 622
          ) {
            weatherIconImg.src = iconUrl + "13d@2x.png";
            this.createSnow();
            this.createTextSnow();
          }
          // Mist [50]
          else if (
            weatherList[0].data.weather[0].id === 701 ||
            weatherList[0].data.weather[0].id === 711 ||
            weatherList[0].data.weather[0].id === 721 ||
            weatherList[0].data.weather[0].id === 731 ||
            weatherList[0].data.weather[0].id === 741 ||
            weatherList[0].data.weather[0].id === 751 ||
            weatherList[0].data.weather[0].id === 761 ||
            weatherList[0].data.weather[0].id === 762 ||
            weatherList[0].data.weather[0].id === 771 ||
            weatherList[0].data.weather[0].id === 781
          ) {
            weatherIconImg.src = iconUrl + "50d@2x.png";
            this.createMist();
            this.createTextMist();
          }
        })
        .catch(e => {
          console.error("Run GetZipCodeWeatherError" + e);
        });
    },
    //郵便番号検索による週間天気取得&アイコン変更処理
    getZipCodeWeekWeather() {
      let searchCity = document.querySelector(
        ".main-contents__input--search-city"
      );
      let searchZip = document.querySelector(
        ".main-contents__input--search-zip"
      );
      let container = document.querySelector(".main-container"); //テキスト挿入
      //明日
      let weekSpot1 = document.querySelector(".weather-week-spot--1");
      let weekMaxTemp1 = document.querySelector(".weather-week-max-temp--1");
      let weekMinTemp1 = document.querySelector(".weather-week-min-temp--1");
      //明後日
      let weekSpot2 = document.querySelector(".weather-week-spot--2");
      let weekMaxTemp2 = document.querySelector(".weather-week-max-temp--2");
      let weekMinTemp2 = document.querySelector(".weather-week-min-temp--2");
      //明々後日
      let weekSpot3 = document.querySelector(".weather-week-spot--3");
      let weekMaxTemp3 = document.querySelector(".weather-week-max-temp--3");
      let weekMinTemp3 = document.querySelector(".weather-week-min-temp--3");

      let weatherIconImg1 = document.querySelector(".weather-icon__img--1");
      let weatherIconImg2 = document.querySelector(".weather-icon__img--2");
      let weatherIconImg3 = document.querySelector(".weather-icon__img--3");
      let weatherList = [];
      let url = "http://api.openweathermap.org/data/2.5/forecast?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";
      this.$axios
        .get(
          url +
            "zip=" +
            searchZip.value +
            ",jp&units=metric&lang=ja&cnt=3&appid=" +
            apiKey
        )
        .then(val => {
          weatherList.push(val);
          container.style.background = "none";
          console.log("Run GetZipCodeWeekWeatherSuccess");
          //テキスト挿入
          //明日
          weekSpot1.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp1.innerHTML =
            Math.round(weatherList[0].data.list[0].main.temp_max) + "℃";
          weekMinTemp1.innerHTML =
            Math.round(weatherList[0].data.list[0].main.temp_min) + "℃";
          //明後日
          weekSpot2.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp2.innerHTML =
            Math.round(weatherList[0].data.list[1].main.temp_max) + "℃";
          weekMinTemp2.innerHTML =
            Math.round(weatherList[0].data.list[1].main.temp_min) + "℃";
          //明々後日
          weekSpot3.innerHTML = weatherList[0].data.city.name;
          weekMaxTemp3.innerHTML =
            Math.round(weatherList[0].data.list[2].main.temp_max) + "℃";
          weekMinTemp3.innerHTML =
            Math.round(weatherList[0].data.list[2].main.temp_min) + "℃";
          //input初期化
          searchCity.value = "";
          searchZip.value = "";
          //アイコン条件分岐 (明日)
          // Clear [01]
          if (weatherList[0].data.list[0].weather[0].id === 800) {
            weatherIconImg1.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[0].weather[0].id === 801) {
            weatherIconImg1.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[0].weather[0].id === 802) {
            weatherIconImg1.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[0].weather[0].id === 803 ||
            weatherList[0].data.list[0].weather[0].id === 804
          ) {
            weatherIconImg1.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[0].weather[0].id === 300 ||
            weatherList[0].data.list[0].weather[0].id === 301 ||
            weatherList[0].data.list[0].weather[0].id === 302 ||
            weatherList[0].data.list[0].weather[0].id === 310 ||
            weatherList[0].data.list[0].weather[0].id === 311 ||
            weatherList[0].data.list[0].weather[0].id === 312 ||
            weatherList[0].data.list[0].weather[0].id === 313 ||
            weatherList[0].data.list[0].weather[0].id === 314 ||
            weatherList[0].data.list[0].weather[0].id === 321 ||
            weatherList[0].data.list[0].weather[0].id === 520 ||
            weatherList[0].data.list[0].weather[0].id === 521 ||
            weatherList[0].data.list[0].weather[0].id === 522 ||
            weatherList[0].data.list[0].weather[0].id === 531
          ) {
            weatherIconImg1.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[0].weather[0].id === 500 ||
            weatherList[0].data.list[0].weather[0].id === 501 ||
            weatherList[0].data.list[0].weather[0].id === 502 ||
            weatherList[0].data.list[0].weather[0].id === 503 ||
            weatherList[0].data.list[0].weather[0].id === 504
          ) {
            weatherIconImg1.src = iconUrl + "10d@2x.png";
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.list[0].weather[0].id === 200 ||
            weatherList[0].data.list[0].weather[0].id === 201 ||
            weatherList[0].data.list[0].weather[0].id === 202 ||
            weatherList[0].data.list[0].weather[0].id === 210 ||
            weatherList[0].data.list[0].weather[0].id === 211 ||
            weatherList[0].data.list[0].weather[0].id === 212 ||
            weatherList[0].data.list[0].weather[0].id === 221 ||
            weatherList[0].data.list[0].weather[0].id === 230 ||
            weatherList[0].data.list[0].weather[0].id === 231 ||
            weatherList[0].data.list[0].weather[0].id === 232
          ) {
            weatherIconImg1.src = iconUrl + "11d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[0].weather[0].id === 511 ||
            weatherList[0].data.list[0].weather[0].id === 600 ||
            weatherList[0].data.list[0].weather[0].id === 601 ||
            weatherList[0].data.list[0].weather[0].id === 602 ||
            weatherList[0].data.list[0].weather[0].id === 611 ||
            weatherList[0].data.list[0].weather[0].id === 612 ||
            weatherList[0].data.list[0].weather[0].id === 613 ||
            weatherList[0].data.list[0].weather[0].id === 615 ||
            weatherList[0].data.list[0].weather[0].id === 616 ||
            weatherList[0].data.list[0].weather[0].id === 620 ||
            weatherList[0].data.list[0].weather[0].id === 621 ||
            weatherList[0].data.list[0].weather[0].id === 622
          ) {
            weatherIconImg1.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[0].weather[0].id === 701 ||
            weatherList[0].data.list[0].weather[0].id === 711 ||
            weatherList[0].data.list[0].weather[0].id === 721 ||
            weatherList[0].data.list[0].weather[0].id === 731 ||
            weatherList[0].data.list[0].weather[0].id === 741 ||
            weatherList[0].data.list[0].weather[0].id === 751 ||
            weatherList[0].data.list[0].weather[0].id === 761 ||
            weatherList[0].data.list[0].weather[0].id === 762 ||
            weatherList[0].data.list[0].weather[0].id === 771 ||
            weatherList[0].data.list[0].weather[0].id === 781
          ) {
            weatherIconImg1.src = iconUrl + "50d@2x.png";
          }
          //アイコン条件分岐 (明後日)
          // Clear [01]
          if (weatherList[0].data.list[1].weather[0].id === 800) {
            weatherIconImg2.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[1].weather[0].id === 801) {
            weatherIconImg2.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[1].weather[0].id === 802) {
            weatherIconImg2.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[1].weather[0].id === 803 ||
            weatherList[0].data.list[1].weather[0].id === 804
          ) {
            weatherIconImg2.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[1].weather[0].id === 300 ||
            weatherList[0].data.list[1].weather[0].id === 301 ||
            weatherList[0].data.list[1].weather[0].id === 302 ||
            weatherList[0].data.list[1].weather[0].id === 310 ||
            weatherList[0].data.list[1].weather[0].id === 311 ||
            weatherList[0].data.list[1].weather[0].id === 312 ||
            weatherList[0].data.list[1].weather[0].id === 313 ||
            weatherList[0].data.list[1].weather[0].id === 314 ||
            weatherList[0].data.list[1].weather[0].id === 321 ||
            weatherList[0].data.list[1].weather[0].id === 520 ||
            weatherList[0].data.list[1].weather[0].id === 521 ||
            weatherList[0].data.list[1].weather[0].id === 522 ||
            weatherList[0].data.list[1].weather[0].id === 531
          ) {
            weatherIconImg2.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[1].weather[0].id === 500 ||
            weatherList[0].data.list[1].weather[0].id === 501 ||
            weatherList[0].data.list[1].weather[0].id === 502 ||
            weatherList[0].data.list[1].weather[0].id === 503 ||
            weatherList[0].data.list[1].weather[0].id === 504
          ) {
            weatherIconImg2.src = iconUrl + "10d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[1].weather[0].id === 511 ||
            weatherList[0].data.list[1].weather[0].id === 600 ||
            weatherList[0].data.list[1].weather[0].id === 601 ||
            weatherList[0].data.list[1].weather[0].id === 602 ||
            weatherList[0].data.list[1].weather[0].id === 611 ||
            weatherList[0].data.list[1].weather[0].id === 612 ||
            weatherList[0].data.list[1].weather[0].id === 613 ||
            weatherList[0].data.list[1].weather[0].id === 615 ||
            weatherList[0].data.list[1].weather[0].id === 616 ||
            weatherList[0].data.list[1].weather[0].id === 620 ||
            weatherList[0].data.list[1].weather[0].id === 621 ||
            weatherList[0].data.list[1].weather[0].id === 622
          ) {
            weatherIconImg2.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[1].weather[0].id === 701 ||
            weatherList[0].data.list[1].weather[0].id === 711 ||
            weatherList[0].data.list[1].weather[0].id === 721 ||
            weatherList[0].data.list[1].weather[0].id === 731 ||
            weatherList[0].data.list[1].weather[0].id === 741 ||
            weatherList[0].data.list[1].weather[0].id === 751 ||
            weatherList[0].data.list[1].weather[0].id === 761 ||
            weatherList[0].data.list[1].weather[0].id === 762 ||
            weatherList[0].data.list[1].weather[0].id === 771 ||
            weatherList[0].data.list[1].weather[0].id === 781
          ) {
            weatherIconImg2.src = iconUrl + "50d@2x.png";
          }
          //アイコン条件分岐 (明々後日)
          // Clear [01]
          if (weatherList[0].data.list[2].weather[0].id === 800) {
            weatherIconImg3.src = iconUrl + "01d@2x.png";
          }
          // Clouds [02]
          else if (weatherList[0].data.list[2].weather[0].id === 801) {
            weatherIconImg3.src = iconUrl + "02d@2x.png";
          }
          // Clouds [03]
          else if (weatherList[0].data.list[2].weather[0].id === 802) {
            weatherIconImg3.src = iconUrl + "03d@2x.png";
          }
          // Clouds [04]
          else if (
            weatherList[0].data.list[2].weather[0].id === 803 ||
            weatherList[0].data.list[2].weather[0].id === 804
          ) {
            weatherIconImg3.src = iconUrl + "04d@2x.png";
          }
          // Rain [09]
          else if (
            weatherList[0].data.list[2].weather[0].id === 300 ||
            weatherList[0].data.list[2].weather[0].id === 301 ||
            weatherList[0].data.list[2].weather[0].id === 302 ||
            weatherList[0].data.list[2].weather[0].id === 310 ||
            weatherList[0].data.list[2].weather[0].id === 311 ||
            weatherList[0].data.list[2].weather[0].id === 312 ||
            weatherList[0].data.list[2].weather[0].id === 313 ||
            weatherList[0].data.list[2].weather[0].id === 314 ||
            weatherList[0].data.list[2].weather[0].id === 321 ||
            weatherList[0].data.list[2].weather[0].id === 520 ||
            weatherList[0].data.list[2].weather[0].id === 521 ||
            weatherList[0].data.list[2].weather[0].id === 522 ||
            weatherList[0].data.list[2].weather[0].id === 531
          ) {
            weatherIconImg3.src = iconUrl + "09d@2x.png";
          }
          // Rain [10]
          else if (
            weatherList[0].data.list[2].weather[0].id === 500 ||
            weatherList[0].data.list[2].weather[0].id === 501 ||
            weatherList[0].data.list[2].weather[0].id === 502 ||
            weatherList[0].data.list[2].weather[0].id === 503 ||
            weatherList[0].data.list[2].weather[0].id === 504
          ) {
            weatherIconImg3.src = iconUrl + "10d@2x.png";
          }
          // Thunderstorm [11]
          else if (
            weatherList[0].data.list[2].weather[0].id === 200 ||
            weatherList[0].data.list[2].weather[0].id === 201 ||
            weatherList[0].data.list[2].weather[0].id === 202 ||
            weatherList[0].data.list[2].weather[0].id === 210 ||
            weatherList[0].data.list[2].weather[0].id === 211 ||
            weatherList[0].data.list[2].weather[0].id === 212 ||
            weatherList[0].data.list[2].weather[0].id === 221 ||
            weatherList[0].data.list[2].weather[0].id === 230 ||
            weatherList[0].data.list[2].weather[0].id === 231 ||
            weatherList[0].data.list[2].weather[0].id === 232
          ) {
            weatherIconImg3.src = iconUrl + "11d@2x.png";
          }
          // Snow [13]
          else if (
            weatherList[0].data.list[2].weather[0].id === 511 ||
            weatherList[0].data.list[2].weather[0].id === 600 ||
            weatherList[0].data.list[2].weather[0].id === 601 ||
            weatherList[0].data.list[2].weather[0].id === 602 ||
            weatherList[0].data.list[2].weather[0].id === 611 ||
            weatherList[0].data.list[2].weather[0].id === 612 ||
            weatherList[0].data.list[2].weather[0].id === 613 ||
            weatherList[0].data.list[2].weather[0].id === 615 ||
            weatherList[0].data.list[2].weather[0].id === 616 ||
            weatherList[0].data.list[2].weather[0].id === 620 ||
            weatherList[0].data.list[2].weather[0].id === 621 ||
            weatherList[0].data.list[2].weather[0].id === 622
          ) {
            weatherIconImg3.src = iconUrl + "13d@2x.png";
          }
          // Mist [50]
          else if (
            weatherList[0].data.list[2].weather[0].id === 701 ||
            weatherList[0].data.list[2].weather[0].id === 711 ||
            weatherList[0].data.list[2].weather[0].id === 721 ||
            weatherList[0].data.list[2].weather[0].id === 731 ||
            weatherList[0].data.list[2].weather[0].id === 741 ||
            weatherList[0].data.list[2].weather[0].id === 751 ||
            weatherList[0].data.list[2].weather[0].id === 761 ||
            weatherList[0].data.list[2].weather[0].id === 762 ||
            weatherList[0].data.list[2].weather[0].id === 771 ||
            weatherList[0].data.list[2].weather[0].id === 781
          ) {
            weatherIconImg3.src = iconUrl + "50d@2x.png";
          }
        })
        .catch(e => {
          console.error("Run GetZipCodeWeekWeatherError..." + e);
        });
    },

    //現在地の週間天気取得&アイコン変更処理
    getCurrentWeekWeather() {
      let lat = null;
      let lon = null;
      //明日
      let weekSpot1 = document.querySelector(".weather-week-spot--1");
      let weekMaxTemp1 = document.querySelector(".weather-week-max-temp--1");
      let weekMinTemp1 = document.querySelector(".weather-week-min-temp--1");
      //明後日
      let weekSpot2 = document.querySelector(".weather-week-spot--2");
      let weekMaxTemp2 = document.querySelector(".weather-week-max-temp--2");
      let weekMinTemp2 = document.querySelector(".weather-week-min-temp--2");
      //明々後日
      let weekSpot3 = document.querySelector(".weather-week-spot--3");
      let weekMaxTemp3 = document.querySelector(".weather-week-max-temp--3");
      let weekMinTemp3 = document.querySelector(".weather-week-min-temp--3");

      let weatherList = [];
      let weatherIconImg1 = document.querySelector(".weather-icon__img--1");
      let weatherIconImg2 = document.querySelector(".weather-icon__img--2");
      let weatherIconImg3 = document.querySelector(".weather-icon__img--3");
      let url = "http://api.openweathermap.org/data/2.5/forecast?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";
      navigator.geolocation.getCurrentPosition(position => {
        lat = position.coords.latitude;
        lon = position.coords.longitude;
        this.$axios
          .get(
            url +
              "lat=" +
              lat +
              "&lon=" +
              lon +
              "&units=metric&lang=ja&cnt=3&appid=" +
              apiKey
          )
          .then(val => {
            weatherList.push(val);
            console.log("Run GetCurrentWeekWeatherSuccess");
            //エラーメッセージ初期化
            this.cityValidMessage1 = false;
            this.cityValidMessage2 = false;
            this.zipValidMessage1 = false;
            this.zipValidMessage2 = false;
            //テキスト挿入
            //明日
            weekSpot1.innerHTML = weatherList[0].data.city.name;
            weekMaxTemp1.innerHTML =
              Math.round(weatherList[0].data.list[0].main.temp_max) + "℃";
            weekMinTemp1.innerHTML =
              Math.round(weatherList[0].data.list[0].main.temp_min) + "℃";
            //明後日
            weekSpot2.innerHTML = weatherList[0].data.city.name;
            weekMaxTemp2.innerHTML =
              Math.round(weatherList[0].data.list[1].main.temp_max) + "℃";
            weekMinTemp2.innerHTML =
              Math.round(weatherList[0].data.list[1].main.temp_min) + "℃";
            //明々後日
            weekSpot3.innerHTML = weatherList[0].data.city.name;
            weekMaxTemp3.innerHTML =
              Math.round(weatherList[0].data.list[2].main.temp_max) + "℃";
            weekMinTemp3.innerHTML =
              Math.round(weatherList[0].data.list[2].main.temp_min) + "℃";
            //アイコン条件分岐 (明日)
            // Clear [01]
            if (weatherList[0].data.list[0].weather[0].id === 800) {
              weatherIconImg1.src = iconUrl + "01d@2x.png";
            }
            // Clouds [02]
            else if (weatherList[0].data.list[0].weather[0].id === 801) {
              weatherIconImg1.src = iconUrl + "02d@2x.png";
            }
            // Clouds [03]
            else if (weatherList[0].data.list[0].weather[0].id === 802) {
              weatherIconImg1.src = iconUrl + "03d@2x.png";
            }
            // Clouds [04]
            else if (
              weatherList[0].data.list[0].weather[0].id === 803 ||
              weatherList[0].data.list[0].weather[0].id === 804
            ) {
              weatherIconImg1.src = iconUrl + "04d@2x.png";
            }
            // Rain [09]
            else if (
              weatherList[0].data.list[0].weather[0].id === 300 ||
              weatherList[0].data.list[0].weather[0].id === 301 ||
              weatherList[0].data.list[0].weather[0].id === 302 ||
              weatherList[0].data.list[0].weather[0].id === 310 ||
              weatherList[0].data.list[0].weather[0].id === 311 ||
              weatherList[0].data.list[0].weather[0].id === 312 ||
              weatherList[0].data.list[0].weather[0].id === 313 ||
              weatherList[0].data.list[0].weather[0].id === 314 ||
              weatherList[0].data.list[0].weather[0].id === 321 ||
              weatherList[0].data.list[0].weather[0].id === 520 ||
              weatherList[0].data.list[0].weather[0].id === 521 ||
              weatherList[0].data.list[0].weather[0].id === 522 ||
              weatherList[0].data.list[0].weather[0].id === 531
            ) {
              weatherIconImg1.src = iconUrl + "09d@2x.png";
            }
            // Rain [10]
            else if (
              weatherList[0].data.list[0].weather[0].id === 500 ||
              weatherList[0].data.list[0].weather[0].id === 501 ||
              weatherList[0].data.list[0].weather[0].id === 502 ||
              weatherList[0].data.list[0].weather[0].id === 503 ||
              weatherList[0].data.list[0].weather[0].id === 504
            ) {
              weatherIconImg1.src = iconUrl + "10d@2x.png";
            }
            // Thunderstorm [11]
            else if (
              weatherList[0].data.list[0].weather[0].id === 200 ||
              weatherList[0].data.list[0].weather[0].id === 201 ||
              weatherList[0].data.list[0].weather[0].id === 202 ||
              weatherList[0].data.list[0].weather[0].id === 210 ||
              weatherList[0].data.list[0].weather[0].id === 211 ||
              weatherList[0].data.list[0].weather[0].id === 212 ||
              weatherList[0].data.list[0].weather[0].id === 221 ||
              weatherList[0].data.list[0].weather[0].id === 230 ||
              weatherList[0].data.list[0].weather[0].id === 231 ||
              weatherList[0].data.list[0].weather[0].id === 232
            ) {
              weatherIconImg1.src = iconUrl + "11d@2x.png";
            }
            // Snow [13]
            else if (
              weatherList[0].data.list[0].weather[0].id === 511 ||
              weatherList[0].data.list[0].weather[0].id === 600 ||
              weatherList[0].data.list[0].weather[0].id === 601 ||
              weatherList[0].data.list[0].weather[0].id === 602 ||
              weatherList[0].data.list[0].weather[0].id === 611 ||
              weatherList[0].data.list[0].weather[0].id === 612 ||
              weatherList[0].data.list[0].weather[0].id === 613 ||
              weatherList[0].data.list[0].weather[0].id === 615 ||
              weatherList[0].data.list[0].weather[0].id === 616 ||
              weatherList[0].data.list[0].weather[0].id === 620 ||
              weatherList[0].data.list[0].weather[0].id === 621 ||
              weatherList[0].data.list[0].weather[0].id === 622
            ) {
              weatherIconImg1.src = iconUrl + "13d@2x.png";
            }
            // Mist [50]
            else if (
              weatherList[0].data.list[0].weather[0].id === 701 ||
              weatherList[0].data.list[0].weather[0].id === 711 ||
              weatherList[0].data.list[0].weather[0].id === 721 ||
              weatherList[0].data.list[0].weather[0].id === 731 ||
              weatherList[0].data.list[0].weather[0].id === 741 ||
              weatherList[0].data.list[0].weather[0].id === 751 ||
              weatherList[0].data.list[0].weather[0].id === 761 ||
              weatherList[0].data.list[0].weather[0].id === 762 ||
              weatherList[0].data.list[0].weather[0].id === 771 ||
              weatherList[0].data.list[0].weather[0].id === 781
            ) {
              weatherIconImg1.src = iconUrl + "50d@2x.png";
            }
            //アイコン条件分岐 (明後日)
            // Clear [01]
            if (weatherList[0].data.list[1].weather[0].id === 800) {
              weatherIconImg2.src = iconUrl + "01d@2x.png";
            }
            // Clouds [02]
            else if (weatherList[0].data.list[1].weather[0].id === 801) {
              weatherIconImg2.src = iconUrl + "02d@2x.png";
            }
            // Clouds [03]
            else if (weatherList[0].data.list[1].weather[0].id === 802) {
              weatherIconImg2.src = iconUrl + "03d@2x.png";
            }
            // Clouds [04]
            else if (
              weatherList[0].data.list[1].weather[0].id === 803 ||
              weatherList[0].data.list[1].weather[0].id === 804
            ) {
              weatherIconImg2.src = iconUrl + "04d@2x.png";
            }
            // Rain [09]
            else if (
              weatherList[0].data.list[1].weather[0].id === 300 ||
              weatherList[0].data.list[1].weather[0].id === 301 ||
              weatherList[0].data.list[1].weather[0].id === 302 ||
              weatherList[0].data.list[1].weather[0].id === 310 ||
              weatherList[0].data.list[1].weather[0].id === 311 ||
              weatherList[0].data.list[1].weather[0].id === 312 ||
              weatherList[0].data.list[1].weather[0].id === 313 ||
              weatherList[0].data.list[1].weather[0].id === 314 ||
              weatherList[0].data.list[1].weather[0].id === 321 ||
              weatherList[0].data.list[1].weather[0].id === 520 ||
              weatherList[0].data.list[1].weather[0].id === 521 ||
              weatherList[0].data.list[1].weather[0].id === 522 ||
              weatherList[0].data.list[1].weather[0].id === 531
            ) {
              weatherIconImg2.src = iconUrl + "09d@2x.png";
            }
            // Rain [10]
            else if (
              weatherList[0].data.list[1].weather[0].id === 500 ||
              weatherList[0].data.list[1].weather[0].id === 501 ||
              weatherList[0].data.list[1].weather[0].id === 502 ||
              weatherList[0].data.list[1].weather[0].id === 503 ||
              weatherList[0].data.list[1].weather[0].id === 504
            ) {
              weatherIconImg2.src = iconUrl + "10d@2x.png";
            }
            // Snow [13]
            else if (
              weatherList[0].data.list[1].weather[0].id === 511 ||
              weatherList[0].data.list[1].weather[0].id === 600 ||
              weatherList[0].data.list[1].weather[0].id === 601 ||
              weatherList[0].data.list[1].weather[0].id === 602 ||
              weatherList[0].data.list[1].weather[0].id === 611 ||
              weatherList[0].data.list[1].weather[0].id === 612 ||
              weatherList[0].data.list[1].weather[0].id === 613 ||
              weatherList[0].data.list[1].weather[0].id === 615 ||
              weatherList[0].data.list[1].weather[0].id === 616 ||
              weatherList[0].data.list[1].weather[0].id === 620 ||
              weatherList[0].data.list[1].weather[0].id === 621 ||
              weatherList[0].data.list[1].weather[0].id === 622
            ) {
              weatherIconImg2.src = iconUrl + "13d@2x.png";
            }
            // Mist [50]
            else if (
              weatherList[0].data.list[1].weather[0].id === 701 ||
              weatherList[0].data.list[1].weather[0].id === 711 ||
              weatherList[0].data.list[1].weather[0].id === 721 ||
              weatherList[0].data.list[1].weather[0].id === 731 ||
              weatherList[0].data.list[1].weather[0].id === 741 ||
              weatherList[0].data.list[1].weather[0].id === 751 ||
              weatherList[0].data.list[1].weather[0].id === 761 ||
              weatherList[0].data.list[1].weather[0].id === 762 ||
              weatherList[0].data.list[1].weather[0].id === 771 ||
              weatherList[0].data.list[1].weather[0].id === 781
            ) {
              weatherIconImg2.src = iconUrl + "50d@2x.png";
            }
            //アイコン条件分岐 (明々後日)
            // Clear [01]
            if (weatherList[0].data.list[2].weather[0].id === 800) {
              weatherIconImg3.src = iconUrl + "01d@2x.png";
            }
            // Clouds [02]
            else if (weatherList[0].data.list[2].weather[0].id === 801) {
              weatherIconImg3.src = iconUrl + "02d@2x.png";
            }
            // Clouds [03]
            else if (weatherList[0].data.list[2].weather[0].id === 802) {
              weatherIconImg3.src = iconUrl + "03d@2x.png";
            }
            // Clouds [04]
            else if (
              weatherList[0].data.list[2].weather[0].id === 803 ||
              weatherList[0].data.list[2].weather[0].id === 804
            ) {
              weatherIconImg3.src = iconUrl + "04d@2x.png";
            }
            // Rain [09]
            else if (
              weatherList[0].data.list[2].weather[0].id === 300 ||
              weatherList[0].data.list[2].weather[0].id === 301 ||
              weatherList[0].data.list[2].weather[0].id === 302 ||
              weatherList[0].data.list[2].weather[0].id === 310 ||
              weatherList[0].data.list[2].weather[0].id === 311 ||
              weatherList[0].data.list[2].weather[0].id === 312 ||
              weatherList[0].data.list[2].weather[0].id === 313 ||
              weatherList[0].data.list[2].weather[0].id === 314 ||
              weatherList[0].data.list[2].weather[0].id === 321 ||
              weatherList[0].data.list[2].weather[0].id === 520 ||
              weatherList[0].data.list[2].weather[0].id === 521 ||
              weatherList[0].data.list[2].weather[0].id === 522 ||
              weatherList[0].data.list[2].weather[0].id === 531
            ) {
              weatherIconImg3.src = iconUrl + "09d@2x.png";
            }
            // Rain [10]
            else if (
              weatherList[0].data.list[2].weather[0].id === 500 ||
              weatherList[0].data.list[2].weather[0].id === 501 ||
              weatherList[0].data.list[2].weather[0].id === 502 ||
              weatherList[0].data.list[2].weather[0].id === 503 ||
              weatherList[0].data.list[2].weather[0].id === 504
            ) {
              weatherIconImg3.src = iconUrl + "10d@2x.png";
            }
            // Thunderstorm [11]
            else if (
              weatherList[0].data.list[2].weather[0].id === 200 ||
              weatherList[0].data.list[2].weather[0].id === 201 ||
              weatherList[0].data.list[2].weather[0].id === 202 ||
              weatherList[0].data.list[2].weather[0].id === 210 ||
              weatherList[0].data.list[2].weather[0].id === 211 ||
              weatherList[0].data.list[2].weather[0].id === 212 ||
              weatherList[0].data.list[2].weather[0].id === 221 ||
              weatherList[0].data.list[2].weather[0].id === 230 ||
              weatherList[0].data.list[2].weather[0].id === 231 ||
              weatherList[0].data.list[2].weather[0].id === 232
            ) {
              weatherIconImg3.src = iconUrl + "11d@2x.png";
            }
            // Snow [13]
            else if (
              weatherList[0].data.list[2].weather[0].id === 511 ||
              weatherList[0].data.list[2].weather[0].id === 600 ||
              weatherList[0].data.list[2].weather[0].id === 601 ||
              weatherList[0].data.list[2].weather[0].id === 602 ||
              weatherList[0].data.list[2].weather[0].id === 611 ||
              weatherList[0].data.list[2].weather[0].id === 612 ||
              weatherList[0].data.list[2].weather[0].id === 613 ||
              weatherList[0].data.list[2].weather[0].id === 615 ||
              weatherList[0].data.list[2].weather[0].id === 616 ||
              weatherList[0].data.list[2].weather[0].id === 620 ||
              weatherList[0].data.list[2].weather[0].id === 621 ||
              weatherList[0].data.list[2].weather[0].id === 622
            ) {
              weatherIconImg3.src = iconUrl + "13d@2x.png";
            }
            // Mist [50]
            else if (
              weatherList[0].data.list[2].weather[0].id === 701 ||
              weatherList[0].data.list[2].weather[0].id === 711 ||
              weatherList[0].data.list[2].weather[0].id === 721 ||
              weatherList[0].data.list[2].weather[0].id === 731 ||
              weatherList[0].data.list[2].weather[0].id === 741 ||
              weatherList[0].data.list[2].weather[0].id === 751 ||
              weatherList[0].data.list[2].weather[0].id === 761 ||
              weatherList[0].data.list[2].weather[0].id === 762 ||
              weatherList[0].data.list[2].weather[0].id === 771 ||
              weatherList[0].data.list[2].weather[0].id === 781
            ) {
              weatherIconImg3.src = iconUrl + "50d@2x.png";
            }
          })
          .catch(e => {
            console.error("Run GetCurrentWeekWeatherError..." + e);
          });
      });
    },
    //現在地の今日の天気取得&アイコン&背景変更処理
    getCurrentWeather() {
      let lat = null;
      let lon = null;
      let spot = document.querySelector(".weather-spot");
      let temp = document.querySelector(".weather-current-temp");
      let maxTemp = document.querySelector(".weather-max-temp");
      let minTemp = document.querySelector(".weather-min-temp");
      let weatherIconImg = document.querySelector(".weather-icon__img");
      let weatherList = [];
      let container = document.querySelector(".main-container");
      let url = "http://api.openweathermap.org/data/2.5/weather?";
      let iconUrl = "http://openweathermap.org/img/wn/";
      let apiKey = "2ff19ba8b889eecaa660b4d8d8821128";
      navigator.geolocation.getCurrentPosition(position => {
        console.log("Position: ", position);
        lat = position.coords.latitude;
        lon = position.coords.longitude;
        this.$axios
          .get(
            url +
              "lat=" +
              lat +
              "&lon=" +
              lon +
              "&units=metric&lang=ja&appid=" +
              apiKey
          )
          .then(val => {
            weatherList.push(val);
            console.log("Run GetCurrentWeatherSuccess");
            //テキスト挿入
            spot.innerHTML = weatherList[0].data.name;
            temp.innerHTML = Math.round(weatherList[0].data.main.temp) + "℃";
            maxTemp.innerHTML =
              Math.round(weatherList[0].data.main.temp_max) + "℃";
            minTemp.innerHTML =
              Math.round(weatherList[0].data.main.temp_min) + "℃";
            container.style.background = "none";
            //エラーメッセージ初期化
            this.cityValidMessage1 = false;
            this.cityValidMessage2 = false;
            this.zipValidMessage1 = false;
            this.zipValidMessage2 = false;
            //アイコン,背景条件分岐
            // Clear [01]
            if (weatherList[0].data.weather[0].id === 800) {
              weatherIconImg.src = iconUrl + "01d@2x.png";
              this.createClear();
              this.createTextClear();
            }
            // Clouds [02]
            else if (weatherList[0].data.weather[0].id === 801) {
              weatherIconImg.src = iconUrl + "02d@2x.png";
              this.createCloud();
              this.createTextCloud();
            }
            // Clouds [03]
            else if (weatherList[0].data.weather[0].id === 802) {
              weatherIconImg.src = iconUrl + "03d@2x.png";
              this.createCloud();
              this.createTextCloud();
            }
            // Clouds [04]
            else if (
              weatherList[0].data.weather[0].id === 803 ||
              weatherList[0].data.weather[0].id === 804
            ) {
              weatherIconImg.src = iconUrl + "04d@2x.png";
              this.createCloud();
              this.createTextCloud();
            }
            // Rain [09]
            else if (
              weatherList[0].data.weather[0].id === 300 ||
              weatherList[0].data.weather[0].id === 301 ||
              weatherList[0].data.weather[0].id === 302 ||
              weatherList[0].data.weather[0].id === 310 ||
              weatherList[0].data.weather[0].id === 311 ||
              weatherList[0].data.weather[0].id === 312 ||
              weatherList[0].data.weather[0].id === 313 ||
              weatherList[0].data.weather[0].id === 314 ||
              weatherList[0].data.weather[0].id === 321 ||
              weatherList[0].data.weather[0].id === 520 ||
              weatherList[0].data.weather[0].id === 521 ||
              weatherList[0].data.weather[0].id === 522 ||
              weatherList[0].data.weather[0].id === 531
            ) {
              weatherIconImg.src = iconUrl + "09d@2x.png";
              this.createRain();
              this.createTextRain();
            }
            // Rain [10]
            else if (
              weatherList[0].data.weather[0].id === 500 ||
              weatherList[0].data.weather[0].id === 501 ||
              weatherList[0].data.weather[0].id === 502 ||
              weatherList[0].data.weather[0].id === 503 ||
              wddeatherList[0].data.weather[0].id === 504
            ) {
              weatherIconImg.src = iconUrl + "10d@2x.png";
              this.createRain();
              this.createTextRain();
            }
            // Thunderstorm [11]
            else if (
              weatherList[0].data.weather[0].id === 200 ||
              weatherList[0].data.weather[0].id === 201 ||
              weatherList[0].data.weather[0].id === 202 ||
              weatherList[0].data.weather[0].id === 210 ||
              weatherList[0].data.weather[0].id === 211 ||
              weatherList[0].data.weather[0].id === 212 ||
              weatherList[0].data.weather[0].id === 221 ||
              weatherList[0].data.weather[0].id === 230 ||
              weatherList[0].data.weather[0].id === 231 ||
              weatherList[0].data.weather[0].id === 232
            ) {
              weatherIconImg.src = iconUrl + "11d@2x.png";
              this.createThunder();
              this.createTextThunder();
            }
            // Snow [13]
            else if (
              weatherList[0].data.weather[0].id === 511 ||
              weatherList[0].data.weather[0].id === 600 ||
              weatherList[0].data.weather[0].id === 601 ||
              weatherList[0].data.weather[0].id === 602 ||
              weatherList[0].data.weather[0].id === 611 ||
              weatherList[0].data.weather[0].id === 612 ||
              weatherList[0].data.weather[0].id === 613 ||
              weatherList[0].data.weather[0].id === 615 ||
              weatherList[0].data.weather[0].id === 616 ||
              weatherList[0].data.weather[0].id === 620 ||
              weatherList[0].data.weather[0].id === 621 ||
              weatherList[0].data.weather[0].id === 622
            ) {
              weatherIconImg.src = iconUrl + "13d@2x.png";
              this.createSnow();
              this.createTextSnow();
            }
            // Mist [50]
            else if (
              weatherList[0].data.weather[0].id === 701 ||
              weatherList[0].data.weather[0].id === 711 ||
              weatherList[0].data.weather[0].id === 721 ||
              weatherList[0].data.weather[0].id === 731 ||
              weatherList[0].data.weather[0].id === 741 ||
              weatherList[0].data.weather[0].id === 751 ||
              weatherList[0].data.weather[0].id === 761 ||
              weatherList[0].data.weather[0].id === 762 ||
              weatherList[0].data.weather[0].id === 771 ||
              weatherList[0].data.weather[0].id === 781
            ) {
              weatherIconImg.src = iconUrl + "50d@2x.png";
              this.createMist();
              this.createTextMist();
            }
          })
          .catch(e => {
            console.error("Run GetCurrentWeatherError..." + e);
          });
      });
    },

    //clearクラス生成
    createClear() {
      //自クラス以外が含まれていたら削除
      let items = document.querySelectorAll(
        "div:not(.cloud,.rain,.snow,.thunder,.mist)"
      );
      if (items) {
        this.clear = true;
        this.cloud = false;
        this.rain = false;
        this.thunder = false;
        this.snow = false;
        this.mist = false;
        console.log("cleateCloudAddClass!");
      }
      console.log("cleateClear!");
    },

    //cloudクラス生成
    createCloud() {
      let items = document.querySelectorAll(
        "div:not(.clear,.rain,.snow,.thunder,.mist)"
      );
      if (items) {
        this.clear = false;
        this.cloud = true;
        this.rain = false;
        this.thunder = false;
        this.snow = false;
        this.mist = false;
      }
      console.log("cleateCloud!");
    },

    //rainクラス生成
    createRain() {
      let items = document.querySelectorAll(
        "div:not(.clear,.cloud,.snow,.thunder,.mist)"
      );
      if (items) {
        this.clear = false;
        this.cloud = false;
        this.rain = true;
        this.thunder = false;
        this.snow = false;
        this.mist = false;
        console.log("cleateCloudAddClass!");
      }
      console.log("cleateRain!");
    },

    //thunderクラス生成
    createThunder() {
      let items = document.querySelectorAll(
        "div:not(.clear,.cloud,.snow,.rain,.mist)"
      );
      if (items) {
        this.clear = false;
        this.cloud = false;
        this.rain = false;
        this.thunder = true;
        this.snow = false;
        this.mist = false;
        console.log("cleateCloudAddClass!");
      }
      console.log("cleateThunder!");
    },

    //snowクラス生成
    createSnow() {
      let items = document.querySelectorAll(
        "div:not(.cloud,.rain,.clear,.thunder,.mist)"
      );
      if (items) {
        this.clear = false;
        this.cloud = false;
        this.rain = false;
        this.thunder = false;
        this.snow = true;
        this.mist = false;
        console.log("cleateCloudAddClass!");
      }
      console.log("cleateSnow!");
    },

    //mistクラス生成
    createMist() {
      let items = document.querySelectorAll(
        "div:not(.cloud,.rain,.snow,.thunder,.clear)"
      );
      if (items) {
        this.clear = false;
        this.cloud = false;
        this.rain = false;
        this.thunder = false;
        this.snow = false;
        this.mist = true;
        console.log("cleateCloudAddClass!");
      }
      console.log("cleateMist!");
    },

    //文章生成(晴れ)
    createTextClear() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は晴れです。";
      textWrap.appendChild(text);
      console.log("createTextClear");
    },

    //文章生成(曇り)
    createTextCloud() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
        console.log("removeText");
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は曇りです。";
      textWrap.appendChild(text);
      console.log("createTextCloud");
    },

    //文章生成(雨)
    createTextRain() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は雨です。外出の際は傘をお持ちください。";
      textWrap.appendChild(text);
      console.log("createTextRain");
    },

    //文章生成(雷)
    createTextThunder() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は雷です。";
      textWrap.appendChild(text);
      console.log("createTextThunder");
    },

    //文章生成(雪)
    createTextSnow() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は雪です。兄元に注意してください。";
      textWrap.appendChild(text);
      console.log("createTextSnow");
    },

    //文章生成(霧)
    createTextMist() {
      let textWrap = document.querySelector(
        ".main-contents__weather-state-message"
      );
      let text = document.createElement("p");
      let target = document.querySelector(".weather-desc-text");
      if (target) {
        target.remove();
      }
      text.classList.add("weather-desc-text");
      text.innerHTML = "今日は霧です。視界に注意してください。";
      textWrap.appendChild(text);
      console.log("createTextMist");
    },

    //バリデーション(都市名検索)
    cityValidation(val) {
      let reg = /[^0-9][a-z]+$/i;
      if (val != "") {
        if (val.match(reg)) {
          this.cityValidMessage1 = false;
          this.cityValidMessage2 = false;
          this.zipValidMessage1 = false;
          this.zipValidMessage2 = false;
          this.searchCity = "";
          return true;
        } else {
          this.cityValidMessage1 = true;
          this.cityValidMessage2 = false;
          this.zipValidMessage1 = false;
          this.zipValidMessage2 = false;
          this.searchCity = "";
          return false;
        }
      } else {
        this.cityValidMessage2 = true;
        this.cityValidMessage1 = false;
        this.zipValidMessage1 = false;
        this.zipValidMessage2 = false;
        return false;
      }
    },

    //バリデーション(郵便番号検索)
    zipValidation(val) {
      let reg = /^[0-9]{3}-[0-9]{4}$/;
      if (val != "") {
        if (val.match(reg)) {
          this.zipValidMessage1 = false;
          this.zipValidMessage2 = false;
          this.cityValidMessage1 = false;
          this.cityValidMessage2 = false;
          this.searchZip = "";
          return true;
        } else {
          this.zipValidMessage1 = true;
          this.zipValidMessage2 = false;
          this.cityValidMessage1 = false;
          this.cityValidMessage2 = false;
          this.searchZip = "";
          return false;
        }
      } else {
        this.zipValidMessage2 = true;
        this.zipValidMessage1 = false;
        this.cityValidMessage1 = false;
        this.cityValidMessage2 = false;
        return false;
      }
    }
  }
};
</script>

<style lang="scss">
.main-container {
  background: #eaeaea;
  height: 100vh;
}

.animation-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  min-height: 100vh;
  z-index: -1;
}

//header
.header {
  display: block;
  padding: 3.5rem 0;
}
.header__title {
  font-size: 3.75rem;
  line-height: 1;
  font-weight: 700;
  text-align: center;
}

//main
.main-contents {
  width: 100%;
      height: 100vh;
  padding: 5rem 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.main-contents__input-area {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.75rem;
  padding-bottom: 2.5rem;
}
.main-contents__form {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2rem;
  align-items: center;
  text-align: center;
  padding-bottom: 2rem;
}
.main-contents__fieldset {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.main-contents__label {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 700;
}
.main-contents__validation-message {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 700;
  color: red;
}
.main-contents__input {
  width: 100%;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
  background-color: "#fff";
  border: none;
  outline: none;
  &:focus {
    outline: none;
  }
}
.main-contents__btn {
  transition-duration: 300ms;
  transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
  transition-property: background-color, border-color, color, fill, stroke,
    opacity, box-shadow, transform, filter, backdrop-filter,
    -webkit-backdrop-filter;
  width: 100%;
  padding: 0.5rem 1.25rem;
  font-weight: 700;
  border-radius: 0.375rem;
  background-color: rgba(59, 130, 246, var(--tw-bg-opacity));
  --tw-bg-opacity: 1;
  color: #fff;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  &:focus,
  &:hover {
    outline: none;
    opacity: 0.7;
  }
}
.main-contents__weather-state-message {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 700;
  padding: 1.25rem 0;
  width: 100%;
  text-align: center;
}
.main-contents__output-area {
  padding-top: 2.5rem;
  row-gap: 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.main-contents__table {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: collapse;
  box-shadow: 0 1px 3px 0px rgb(0 0 0 / 50%);
  padding: 1.25rem;
  border-radius: 0.375rem;
  background-color: rgba(150, 150, 150, 1);
}
.main-contents__tr--date {
  height: 3rem;
  border-bottom-width: 1px;
  border-top-left-radius: 0.375rem;
}
.main-contents__th--date {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 700;
  border-top-left-radius: 0.375rem;
  color: #fff;
}
.main-contents__tr--sort {
  height: 5rem;
  text-align: center;
  color: #fff;
}
.main-contents__tr--sort--first,
.main-contents__tr--sort--second {
  border-bottom: 1px solid #fff;
}
.main-contents__th--sort,
.main-contents__td {
  width: 20%;
  color: #fff;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  line-height: 1.5rem;
  font-weight: 700;
  &:not(:last-child) {
    border-right: 1px solid #fff;
  }
}
.main-contents__td-icon {
  margin: 0 auto;
}

//天気クラス
.clear {
  background: linear-gradient(330deg, #1f2102, #38c2f7, #a1d7fd, #ffffff);
  background-size: 400% 400%;
  animation: clear-anim 7s infinite ease;
}

.cloud {
  background: linear-gradient(330deg, #1f2102, #9a9c9c, #e4e4e4, #dddfe0);
  background-size: 400% 400%;
  animation: cloud-anim 7s infinite ease;
}

.rain {
  background: linear-gradient(330deg, #1f2102, #326eb7, #80b6ef, #dddfe0);
  background-size: 400% 400%;
  animation: cloud-anim 3s infinite ease;
}

.thunder {
  background: linear-gradient(330deg, #1f2102, #414986, #d6daa3, #deded5);
  background-size: 400% 400%;
  animation: thunder-anim 1s infinite ease;
}

.snow {
  background: linear-gradient(330deg, #1f2102, #28a3c7, #babdbd, #dddfe0);
  background-size: 400% 400%;
  animation: snow-anim 6s infinite ease;
}

.mist {
  background: linear-gradient(330deg, #1f2102, #597d88, #c1c1c1, #dddfe0);
  background-size: 400% 400%;
  animation: mist-anim 7s infinite ease;
}

.rain + .main-contents {
  color: "#fff";
}

//animation
//clear
@keyframes clear-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

//cloud
@keyframes cloud-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

//rain
@keyframes rain-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

//thunder
@keyframes thunder-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

//snow
@keyframes snow-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

//mist
@keyframes mist-anim {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

@media screen and(min-width: 768px) {
  .header,
  .main-contents {
    width: 60%;
    margin: 0 auto;
  }
}
</style>
