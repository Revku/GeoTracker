<template>
  <div class="home">
      <h2 class="home__title">Geo<span>Tracker</span></h2>

      <div class="home__box" v-if="!result || result.status != 200">
      <input type="text" class="home__input" placeholder="Country Name" @keyup.enter="geoTrack()">
      <p style="color: red; margin-top: 20px;" class="home__errorbox" v-if="errorMessage">{{ errorMessage }}</p>
      <button class="home__button" @click="geoTrack()">Go track!</button>
      </div>

      <div class="result" v-if="result.status == 200">
        <h2 class="result__title">{{ info.name }} ({{ info.nativeName }})</h2>
        
        <div class="result__itembox">
        <p class="result__label">Region</p>
        <p class="result__item">{{ info.subregion }}</p>
        </div>

        <div class="result__itembox">
        <p class="result__label">Population</p>
        <p class="result__item">{{ info.population }}</p>
        </div>
        
        <div class="result__itembox">
        <p class="result__label">Capital</p>
        <p class="result__item">{{ info.capital }}</p>
        </div>

        <div class="result__itembox">
        <p class="result__label">Language</p>
        <p class="result__item">{{ info.languages[0].name }}</p>
        </div>

        <div class="result__itembox">
        <p class="result__label">Currency</p>
        <p class="result__item">{{ info.currencies[0].name }}</p>
        </div><br>

        <button class="result__button" @click="backToHomePage()">Back to homepage</button>
      </div>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
  data() {return {
    result: '',
    info: '',
    errorMessage: '',
  }},
  methods: {
    geoTrack() {
      const country = document.querySelector('.home__input').value;
      if (!country) {
        this.$data.errorMessage = "Field is empty!"
        return 0;
      }
      
      const url = `https://restcountries.eu/rest/v2/name/${country}`;
      axios.get(url)
        .then(res => {
            this.$data.result = res;
            this.$data.info = res.data[0];
            console.log(res);
        })
        .catch(() => {
          this.$data.errorMessage = "Country not found!"
        })
    },
    backToHomePage() {
      location.reload()
    }
  },
  }
</script>

<style lang="scss" scoped>
.home {
  padding: 20px;
  &__title {
    font-size: 45px;
    font-weight: 500;
    margin-bottom: 50px;

    span {
      font-weight: 600;
      color: #FFC700;
    }
  }

  &__input {
    background: none;
    border: 2px solid #8a8a8a;
    text-align: center;
    border-radius: 50px;
    padding: 10px 20px;
    display: block;
    font-size: 17px;
    outline: none;
    width: 100%;
    color: white;
  }

  &__button {
    background-color: #FFC700;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 10px 40px;
    border-radius: 50px;
    margin-top: 30px;
    font-size: 15px;
  }

}

.result {
  &__title {
    font-size: 30px;
    margin-bottom: 20px;
  }
  &__itembox {
    display: inline-block;
    margin-left: 20px;
  }

  &__label {
    font-size: 14px;
    color: #FFC700;
    margin-top: 20px;
  }

  &__item {
    font-size: 20px;
    font-weight: 600;
  }

    &__button {
      background: none;
      border: 2px gray solid;
      color: gray;
      outline: none;
      cursor: pointer;
      padding: 10px 40px;
      border-radius: 50px;
      margin-top: 50px;
      font-size: 15px;
      text-align: center;

      &:hover {
        border: 2px white solid;
        color: white;
      }
  }
}
</style>