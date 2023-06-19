<script>
  import Chart from "chart.js/auto";
  import { formatMonth } from "../lib/formatters";

  export let transactions = [];

  const data = transactions.reduce((acc, it) => {
    const date = new Date(it.date);
    const month = formatMonth(date);

    if (!acc[month]) {
      acc[month] = {};
    }

    if (!acc[month][it.type]) {
      acc[month][it.type] = 0;
    }

    acc[month][it.type] += it.amount;

    return acc;
  }, {});

  const chartAction = (context) => {
    const chart = new Chart(context, {
      type: "line",
      data: {
        labels: Object.keys(data),
        datasets: Object.values(data).reduce(
          (acc, it) => {
            acc[0].data.push(it.income);
            acc[1].data.push(it.expense);
            return acc;
          },
          [
            { label: "Income", data: [] },
            { label: "Expenses", data: [] },
          ]
        ),
      },
      options: {
        scales: {
          y: {
            beginAtZero: true,
          },
        },
      },
    });
    return chart.destroy.bind(chart);
  };
</script>

<canvas use:chartAction />

<style>
  canvas {
    margin: 3rem 0;
  }
</style>
