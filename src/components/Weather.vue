<template>
  <main class="main">
    <!-- @submit.prevent is equal to event.preventDefault(); in the method -->
    <form action="" @submit.prevent="getWeather" class="form">
      <input type="text" placeholder="Ex: Annecy" v-model.lazy="cityRequested" class="form__input">
    </form>
    <p v-if="errorMsg" class="errorMsg">{{ errorMsg }}</p>
    <section class="results" v-if="cityInfos !== null">
      <h2 class="results__title">Prévisions météo pour <span class="results__title__cityName">{{ cityRequested }}</span>.</h2>
      <div class="results__card">
        <p class="results__card__temperature">{{ cityInfos.temperature }}</p>
        <div class="results__card__infos">
          <p class="results__card__infos__oneinfo">{{ cityInfos.description }}</p>
          <div class="results__card__infos__wind">
            <p class="results__card__infos__wind__svg">
              <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-wind" width="35" height="35" viewBox="0 0 24 24" stroke-width="1.5" stroke="#003A64" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M5 8h8.5a2.5 2.5 0 1 0 -2.34 -3.24" />
                <path d="M3 12h15.5a2.5 2.5 0 1 1 -2.34 3.24" />
                <path d="M4 16h5.5a2.5 2.5 0 1 1 -2.34 3.24" />
              </svg>
            </p>
            <p class="results__card__infos__oneinfo">{{ cityInfos.wind }}</p>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  name: "Weather",
  data() {
    return {
      cityRequested: "",
      errorMsg: "",
      cityInfos: null,
    }
  },
  methods: {
    getWeather() {
      /* Checking if the request from the user is empty */
      if(this.cityRequested === "") {
        this.errorMsg = "Veuillez entrer le nom d'une ville !";
        //console.log(this.errorMsg);

        // Put back the cityInfos object to null to deal with UI errors 
        this.cityInfos = null;
      } else {
        // Query must be : goweather.herokuapp.com/weather/nameOfTheCity
        // So we concatenate w/ the cityRequested content
        axios.get('https://goweather.herokuapp.com/weather/' + this.cityRequested)
          .then((response) => {
            // console.log(response.data);
            this.cityInfos = response.data;
            // Empty the error message, in case this one contains a message from a previous search 
            this.errorMsg = "";
          })
          .catch((error) => {
            //console.log(error);

            // HTTP response is 404 (Not found)
            // Warn the user that the API's data didn't found a match for their city request 
            if(error.response.status === 404) {
              this.errorMsg = "La ville de " + this.cityRequested + " n'a pas été trouvée !";

              // Put back the cityInfos object to null to deal with UI errors 
              this.cityInfos = null;
            }
          })
      }
    }
  }
}
</script>

<style lang="scss">
@import '../scss/vars.scss';

.main {
  padding: $gutter;
}

.errorMsg {
  text-align: center;
  margin: $gutter * 2 0;
  color: $navyBlue;
}

.form {
  display: flex;
  justify-content: center;
  
  &__input {
    width: 40%;
    height: 2rem;
    border: $tealGrenne solid 2px; 
    border-radius: 25px;
    padding: 0 1rem;
    color: $tealGrenne;
  }
}

.results {
  margin: $gutter * 2 0;
  padding: $gutter * 2;
  background-color: $darkerMint;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  color: $navyBlue;

  &__title {
    text-align: center;
    width: 100%;

    &__cityName {
      font-weight: $titleWeight;
      text-transform: capitalize;
      font-size: $nameSize;
    }
  }

  &__card {
    padding: $gutter * 2;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;

    &__temperature {
      color: $darkerMint;
      font-weight: $titleWeight;
      font-size: 5rem;
      margin-bottom: $gutter * 2;
      padding: $gutter * 3;
      background-color: $navyBlue;
      border: $navyBlue 3px solid;
      border-radius: 25px;
    }

    &__infos {
      border: $navyBlue 3px solid;
      background-color: $white;

      margin-bottom: $gutter * 2;

      &__wind {
        display: flex;
        justify-content: flex-start;
        align-items: center;

        &__svg {
          width: 100%;
          margin: $gutter;
        }
      }

      &__oneinfo {
        width: 100%;
        margin: $gutter;
      }
    }
  }

}

</style>