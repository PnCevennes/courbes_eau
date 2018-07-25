<template>
    <div id="app">
        <div class="titre">
            <img src="static/logo_pnc_orange.png" />
            <h1 class="sel_titre">Visualisation des données de Fluorimètre</h1>
        </div>
        <div id="selecteur">
            <file-select v-model="data" />
            <label id="filter_label">Echantillonnage 1/<input type="number" v-model="filtre" min="1" /></label>
        </div>
        <div id="afficheur" v-if="data.length">
            <line-chart v-if="data.length" :data="chartData" :options="{responsive: false, hover: {mode: 'nearest', intersect: true}}" :height="150" />
            <data-table v-model="data" />
        </div>
    </div>
</template>
<script>
import fileSelect from '@/components/fileselect'
import dataTable from '@/components/datatable'
import lineChart from '@/components/linechart'

const SENSORS = [
    {id: 3, color: 'rgba(250,120,120,0.8)', label: 'Tracer 1'},
    {id: 4, color: 'rgba(120,250,120,0.8)', label: 'Tracer 2'},
    {id: 5, color: 'rgba(120,120,250,0.8)', label: 'Tracer 3'},
    {id: 6, color: 'rgba(180,250,120,0.8)', label: 'Turbidity'},
    {id: 7, color: 'rgba(120,250,180,0.8)', label: 'Baseline'},
    {id: 8, color: 'rgba(120,180,250,0.8)', label: 'Battery'},
    {id: 9, color: 'rgba(250,180,120,0.8)', label: 'Temperature'}
]

export default {
    name: 'App',
    components: {
        fileSelect,
        dataTable,
        lineChart
    },
    computed: {
        items () {
            if (this.data.length) {
                var items = this.data.slice()
                return items.filter(x => (parseInt(x[0]) % parseInt(this.filtre)) === 0)
            }
        },
        chartData () {
            if (this.items.length) {
                var datasets = []
                var labels = this.items.map(x => x[1]).filter(x => !!x)
                SENSORS.forEach(model => {
                    datasets.push({
                        label: model.label,
                        backgroundColor: model.color,
                        borderColor: model.color,
                        pointBorderWidth: 0,
                        pointRadius: 0,
                        fill: false,
                        data: this.items.map(x => x[model.id]).filter(x => !!x)
                    })
                })
                return {
                    options: {
                        responsive: true,
                        tooltips: {
                            mode: 'index',
                            intersect: false
                        },
                        hover: {
                            mode: 'nearest',
                            intersect: true
                        },
                        elements: {
                            line: {
                                tension: 0
                            }
                        }
                    },
                    labels,
                    datasets
                }
            }
            return {}
        }
    },
    data () {
        return {
            data: '',
            filtre: 10
        }
    }
}
</script>
