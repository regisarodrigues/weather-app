<script lang="ts">
  import type { IWeather } from 'src/interfaces/IWeather';
  import { API_KEY, URL_API, LANG } from '../utils/variaveis';
  import ErrorAlert from './ErrorAlert.svelte';
  import Loading from './Loading.svelte';
  import WeatherData from './WeatherData.svelte';

  let CITY_NAME: string = '';

  const getWeatherData = async (CITY_NAME: string = 'Franca') => {
    const baseURL: string = `${URL_API}/weather?q=${CITY_NAME}&units=metric&lang=${LANG}&appid=${API_KEY}`;

    if (CITY_NAME.trim() === '') {
      throw new Error('É nessesário informar uma cidade.');
    }

    const res: Response = await fetch(baseURL);

    if (!res.ok) {
      throw new Error('Cidade não encontrada.');
    }

    const data: IWeather = await res.json();
    return data;
  };

  let data = getWeatherData();

  const handleSubmit = () => {
    data = getWeatherData(CITY_NAME);
    CITY_NAME = '';
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
      <Loading />
    {:then weather}
      <WeatherData {weather} />
    {:catch err}
      <ErrorAlert {err} />
    {/await}
  </div>
</div>
