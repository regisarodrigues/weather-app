<script lang="ts">
  import type { IWeather } from 'src/interfaces/IWeather';

  export let weather: IWeather;
</script>

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
    Cidade: <span class="fw-bold">{weather.name}, {weather.sys.country}</span>
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
