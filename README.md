# Svelte Apex Charts

Create and update dynamic SVG charts in Svelte(Kit) using [ApexCharts] - [demo]

[ApexCharts]: https://apexcharts.com/
[demo]: https://svelte.dev/repl/5c0f420535964b4da65c03c6bcb69439?version=4.2.8

## Quick-Start

```shell
pnpm add apexcharts svelte-apex-charts -D
npm i apexcharts svelte-apex-charts -D
# Bring your own ApexCharts library.
# It is listed as a peer dependency.
```

```svelte
<script>
  import { renderChart } from 'svelte-apex-charts';

  /** @type {import('svelte-apex-charts').Chart} */
  const chart = {
    options: {
      // Provide your own ApexChars options.
      chart: { type: 'bar' },
      series: [{ name: 'sales', data: [5, 6, 13, 26] }],
      xaxis: { categories: [2020, 2021, 2022, 2023] }
    }
  };
</script>

<!-- After the chart is created, chart.ref and chart.node are set. -->
<!-- This allows all ApexCharts methods to be used. (e.g. dataURI) -->
<div use:renderChart={chart}></div>

<!-- When the <div> element is unmounted, the chart gets destroyed. -->

<button
  type="button"
  on:click={() => {
    // chart.node is also available, which points to the <div> element.
    chart.ref?.updateSeries([{ data: [7, 10, 20, 23] }]);
  }}
>
  Update Series
</button>
```

## Features

Compared to the [svelte-apexcharts] library,

- Supports SvelteKit - no more [window is not defined] error.
- Supports TypeScript - be confident with the chart options.
- Provides the chart reference - all Apex [methods] can be used.

[svelte-apexcharts]: https://github.com/galkatz373/svelte-apexcharts#readme
[window is not defined]: https://github.com/galkatz373/svelte-apexcharts/issues/163
[methods]: https://apexcharts.com/docs/methods

```javascript
chart.ref?.updateOptions({ xaxis: { labels: { show: false } }, yaxis: { min: 20, max: 100 } });
chart.ref?.addXaxisAnnotation({ x: 40, label: { text: 'Lorem Ipsum' } });
// And many more. Reference the ApexCharts documentation.
```

## Notes

### The Chart Object

- Create a chart object and pass the reference as the action parameter.
- This is necessary to update and provide `chart.ref` and `chart.node`.

```svelte
<div use:renderChart={chart}></div>
```

### Server-Side Rendering

- ApexCharts does not support SSR, and the [issue] is marked as stale.
- This StackOverflow [post] shows other ways of handling SSR errors.

[issue]: https://github.com/apexcharts/apexcharts.js/issues/3703
[post]: https://stackoverflow.com/questions/69874742
