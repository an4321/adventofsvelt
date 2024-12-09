<script lang="ts">
    import { Chart, registerables } from "chart.js";

    Chart.register(...registerables);

    let chart: any;

    let { data } = $props();
    // Reduce data to unique names with total times
    const totalTimeByPerson = data.reduce((acc, current) => {
        if (!acc[current.elf]) {
            acc[current.elf] = 0;
        }
        acc[current.elf] += current.minutesTaken;
        return acc;
    }, {});

    // Convert object to list
    const chart_data = Object.keys(totalTimeByPerson).map((name) => ({
        name,
        time: totalTimeByPerson[name],
    }));

    $effect(async () => {
        const ctx = document.getElementById("chart").getContext("2d");
        chart = new Chart(ctx, {
            type: "bar",
            data: {
                labels: chart_data.map((item) => item.name),
                datasets: [
                    {
                        label: "Total time in min",
                        data: chart_data.map((item) => item.time),
                        backgroundColor: [
                            "rgba(255, 99, 132, 0.2)",
                            "rgba(54, 162, 235, 0.2)",
                            "rgba(255, 206, 86, 0.2)",
                            "rgba(75, 192, 192, 0.2)",
                            "rgba(153, 102, 255, 0.2)",
                        ],
                        borderColor: [
                            "rgba(255, 99, 132, 1)",
                            "rgba(54, 162, 235, 1)",
                            "rgba(255, 206, 86, 1)",
                            "rgba(75, 192, 192, 1)",
                            "rgba(153, 102, 255, 1)",
                        ],
                        borderWidth: 1,
                    },
                ],
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                    },
                },
            },
        });
    });
</script>

<h2>Work done by each elf</h2>
<canvas id="chart"></canvas>
