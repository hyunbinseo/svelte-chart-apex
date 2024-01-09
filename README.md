# Svelte Chart Apex

Create and update dynamic SVG charts in Svelte(Kit) using [ApexCharts] - [demo]

[ApexCharts]: https://apexcharts.com/
[demo]: https://svelte.dev/repl/5c0f420535964b4da65c03c6bcb69439?version=4.2.8

## Features

Compared to the [svelte-apexcharts] library:

✅ SvelteKit Support ― no more [window is not defined] error.\
✅ TypeScript Support ― be confident with the chart options.\
✅ Chart Reference ― all ApexCharts' [methods] can be used.

[svelte-apexcharts]: https://github.com/galkatz373/svelte-apexcharts#readme
[window is not defined]: https://github.com/galkatz373/svelte-apexcharts/issues/163
[methods]: https://apexcharts.com/docs/methods

```javascript
chart.ref?.updateOptions({ xaxis: { labels: { show: false } }, yaxis: { min: 20, max: 100 } });
chart.ref?.addXaxisAnnotation({ x: 40, label: { text: 'Lorem Ipsum' } });
// And many more. Reference the ApexCharts documentation.
```

## Quick-Start

```shell
pnpm add apexcharts svelte-chart-apex -D
npm i apexcharts svelte-chart-apex -D
# Bring your own ApexCharts library.
# It is listed as a peer dependency.
```

SvelteKit example. For a Svelte-only example, reference the [demo] REPL.

```svelte
<script lang="ts">
  import { browser } from '$app/environment';
  import { renderChart, type Chart } from 'svelte-chart-apex';

  export let data;

  const chart: Chart = {
    // Provide your own ApexCharts options.
    options: {
      chart: { type: 'bar' },
      xaxis: { categories: [2020, 2021, 2022, 2023] },
      series: data.series // Data from the initial page load.
    }
    // These key-values are later set by the action.
    // - node: HTMLDivElement; - No need to use bind:this on the <div> element.
    // - ref: ApexCharts; - Use the ApexCharts' methods. (e.g. updateOptions)
  };

  $: if (browser && data) {
    // Update the chart when the page data gets updated. (e.g. invalidateAll)
    // The `updateSeries` does not run on mount, since the `ref` is undefined.
    chart.ref?.updateSeries(data.series);
  }
</script>

<!-- When this HTML element is unmounted, the chart is automatically destroyed. -->
<div use:renderChart={chart}></div>

<button
  type="button"
  on:click={() => {
    // Since chart.ref can be undefined, it has to be checked.

    // Option 1 - Optional Chaining
    chart.ref?.updateSeries([{ data: [7, 10, 20, 23] }]);

    // Option 2 - Simple if check
    if (chart.ref) chart.ref.updateSeries([{ data: [7, 10, 20, 23] }]);
  }}
>
  Update Series
</button>
```

## Server-Side Rendering

- ApexCharts does not support SSR, and the [issue] is marked as stale.
- This StackOverflow [post] shows other ways of handling SSR errors.

[issue]: https://github.com/apexcharts/apexcharts.js/issues/3703
[post]: https://stackoverflow.com/questions/69874742
