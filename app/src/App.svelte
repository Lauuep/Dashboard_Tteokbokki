<script>
  import { onMount } from 'svelte';
  import { getSummary, getPoints } from './lib/api';
  import BarChart from './lib/components/BarChart.svelte';
  import ScatterPlot from './lib/components/ScatterPlot.svelte';

  let loading = true;
  let error = '';
  let summary = null;
  let points = [];

  onMount(async ()=>{
    try{
      summary = await getSummary();
      points = await getPoints(800);
    }catch(e){
      error = e.message || 'Error cargando datos';
    }finally{
      loading = false;
    }
  });
</script>

<div class="max-w-7xl mx-auto p-6 space-y-6">
  <header class="flex items-end justify-between">
    <div>
      <h1 class="text-2xl font-bold">Ascensor Social — Dashboard</h1>
      <p class="text-slate-600">Svelte + D3 + Tailwind &middot; API: <code>{typeof __API_URL__ !== 'undefined' ? __API_URL__ : 'http://localhost:3000'}</code></p>
    </div>
  </header>

  {#if loading}
    <div class="animate-pulse text-slate-500">Cargando datos...</div>
  {:else if error}
    <div class="text-red-600">⚠ {error}</div>
  {:else}
    <section class="grid md:grid-cols-4 gap-4">
      <div class="bg-pink-200 p-4 rounded-2xl shadow">
        <div class="text-slate-500 text-sm">muestras</div>
        <div class="text-3xl font-semibold">{summary.n}</div>
      </div>
      <div class="bg-blue-200 p-4 rounded-2xl shadow">
        <div class="text-slate-500 text-sm">correlación (ingresos padres-hijo)</div>
        <div class="text-3xl font-semibold">{summary.corr}</div>
      </div>
      <div class="bg-green-200 p-4 rounded-2xl shadow">
        <div class="text-slate-500 text-sm">regiones</div>
        <div class="text-3xl font-semibold">{summary.regionMobility.length}</div>
      </div>
      <div class="bg-yellow-200 p-4 rounded-2xl shadow">
        <div class="text-slate-500 text-sm">regiones</div>
        <div class="text-3xl font-semibold">{summary.regionMobility.length}</div>
      </div>
    </section>

    <section class="grid md:grid-cols-3 gap-6">

      <div class="rounded-lg bg-white p-4 rounded-2xl shadow">
        <div class="relative overflow-hidden bg-pink-200">
          <BarChart data={summary.regionMobility.map(d=>({label:d.region, value:d.movilidad}))} />
        </div>
        <div class="p-4 text-surface">
          <h5 class="mb-2 text-xl font-medium leading-tight">Índice simple de movilidad por región</h5>
          <p class="text-black"> Some quick example text to build on the card title and make up the bulk of the card's content.</p>
        </div>
      </div>

      <div class="rounded-lg bg-white p-4 rounded-2xl shadow">
        <div class="relative overflow-hidden bg-blue-200">
          <ScatterPlot data={points}/>
        </div>
        <div class="p-4 text-surface">
          <h5 class="mb-2 text-xl font-medium leading-tight">Ingresos: padres vs hijo (con regresión)</h5>
          <p class="text-black"> Some quick example text to build on the card title and make up the bulk of the card's content.</p>
        </div>
      </div>

      <div class="rounded-lg bg-white bg-pink-500 p-4 rounded-2xl shadow">
        <div class="relative overflow-hidden bg-yellow-200">
          <BarChart data={summary.regionMobility.map(d=>({label:d.region, value:d.movilidad}))} />
        </div>
        <div class="p-6 text-surface">
          <h5 class="mb-2 text-xl font-medium leading-tight">Índice simple de movilidad por región</h5>
          <p class="text-black"> Some quick example text to build on the card title and make up the bulk of the card's content.</p>
        </div>
      </div>

    </section>

    <section class="grid md:grid-cols-2 gap-8">
      <div class="flex flex-col rounded-lg bg-white text-surface shadow md:max-w-xl md:flex-row">
        <BarChart data={summary.regionMobility.map(d=>({label:d.region, value:d.movilidad}))} /> 
        <div class="flex flex-col justify-start p-6">
          <h5 class="mb-2 text-xl font-medium">Card title</h5>
          <p class="mb-4 text-base"> This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.</p>
        </div>
      </div>

      <div class="flex flex-col rounded-lg bg-white text-surface shadow md:max-w-xl md:flex-row bg-orange-200">
        <BarChart data={summary.regionMobility.map(d=>({label:d.region, value:d.movilidad}))} /> 
        <div class="flex flex-col justify-start p-6">
          <h5 class="mb-2 text-xl font-medium">Card title</h5>
          <p class="mb-4 text-base"> This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.</p>
        </div>
      </div>
    </section>



    <footer class="text-sm text-slate-500">
      Dataset sintético para docencia; sustituye la API por tus datos reales cuando quieras.
    </footer>
  {/if}
</div>
