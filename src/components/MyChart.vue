<script>
import { Line } from 'vue-chartjs';
import 'chartjs-plugin-streaming';

export default {
    extends: Line,
    data() {
        return {
            datas: {
                'cpu': 0,
                'memory': 0,
            }
        }
    },
    mounted() {
        this.renderChart({
            datasets: [{
                name: 'cpu',
                label: 'cpu使用率',
                borderColor: 'rgb(255, 99, 132)',
                backgroundColor: 'rgba(255, 99, 132, 0.5)',
                fill: false,
                cubicInterpolationMode: 'monotone',
                data: []
            }, {
                name: 'memory',
                label: 'memory使用率',
                borderColor: 'rgb(54, 162, 235)',
                backgroundColor: 'rgba(54, 162, 235, 0.5)',
                fill: false,
                cubicInterpolationMode: 'monotone',
                data: []
            }]
        }, {
            title: {
                display: true,
                text: 'test Graph',
            },
            scales: {
                xAxes: [{
                    type: 'realtime',
                    realtime: {
                        duration: 20000,
                        refresh: 2000,
                        delay: 2000,
                        onRefresh: (chart) => {
                            chart.data.datasets.forEach((dataset) => {
                                dataset.data.push({
                                    x: Date.now(),
                                    y: this.GetCPU(dataset.name)
                                });
                            });
                        },
                    }
                }],
                yAxes: [{
                    scaleLabel: {
                        display: true,
                        labelString: 'value'
                    }
                }]
            },
            tooltips: {
                mode: 'nearest',
                intersect: false
            },
            hover: {
                mode: 'nearest',
                intersect: false
            },
            responsive: true
        });
    },
    methods: {
        GetCPU(name) {
            //let data = this.datas.find(data => data.name === name);
            this.$axios
                .get('http://127.0.0.1:8000/', {
                    params: {
                        type: name
                    }
                })
                .then(response => {
                    this.datas[name] = response.data[name]
                })
                .catch(e => {
                    console.log(e)
                })
            console.log(this.datas[name])
            return this.datas[name]
        },
    }
}
</script>

<style>
.chartjs-render-monitor {
    width: 1000px !important;
    height: 700px !important;
    margin: auto;
}
</style>