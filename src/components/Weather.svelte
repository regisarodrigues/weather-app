<script lang="ts">
  import type { IWeather } from 'src/interfaces/IWeather';
  import { API_KEY, URL_API, LANG } from '../utils/variaveis';

  let CITY_NAME = 'Franca';

  let url = `${URL_API}/weather?q=${CITY_NAME}&units=metric&lang=${LANG}&appid=${API_KEY}`;

  async function getWeatherData<T>(request: RequestInfo): Promise<T> {
    const response = await fetch(request);
    const data = await response.json();
    return data;
  }

  let data = getWeatherData<IWeather>(url);

  const handleSubmit = () => {
    url = `${URL_API}/weather?q=${CITY_NAME}&units=metric&lang=${LANG}&appid=${API_KEY}`;
    data = getWeatherData<IWeather>(url);
  };
</script>

<div class="container">
  <div class="row">
    <div class="col-12 mb-3">
      <div class="col-6 container-fluid">
        <form
          class="d-flex justify-content-center"
          autocomplete="off"
          on:submit|preventDefault={handleSubmit}
        >
          <input
            bind:value={CITY_NAME}
            class="form-control me-2"
            type="text"
            placeholder="Cidade"
          />
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>

    {#await data}
      <div class="text-center">
        <div
          class="spinner-border text-primary"
          style="width: 4rem; height: 4rem; font: bold;"
          role="status"
        />
      </div>
    {:then weather}
      <div class="col-12 col-md-6">
        <h1>
          <img
            style="background-color: #8cb3d9;"
            src={` http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`}
            alt={weather.weather[0].description}
          />
          {weather.main.temp.toFixed()}°C
        </h1>
        <h4>
          Cidade: <span class="fw-bold"
            >{weather.name}, {weather.sys.country}</span
          >
        </h4>
        <h4>
          Tempo agora: <span class="fw-bold text-capitalize"
            >{weather.weather[0].description}</span
          >
        </h4>
      </div>
      <div class="col-12 col-md-6">
        <h4>
          Temp. Maxima: <span class="fw-bold text-danger"
            >{weather.main.temp_max.toFixed()}°C</span
          >
        </h4>
        <h4>
          Temp. Minima: <span class="fw-bold text-primary"
            >{weather.main.temp_min.toFixed()}°C</span
          >
        </h4>
      </div>
    {/await}
  </div>
</div>
