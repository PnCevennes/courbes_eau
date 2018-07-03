<template>
    <div id="app">
        <file-select v-model="data" />
        <line-chart v-if="data.length" :data="chartData" :height="100" />
        <data-table v-model="data" />
    </div>
</template>
<script>
import fileSelect from '@/components/fileselect'
import dataTable from '@/components/datatable'
import lineChart from '@/components/linechart'

export default {
    name: 'App',
    components: {
        fileSelect,
        dataTable,
        lineChart
    },
    computed: {
        chartData () {
            if (this.data.length) {
                var items = this.data.slice(1, this.data.length)
                var labels = items.map(x => x[0]).filter(x => !!x)
                var values = items.map(x => x[1]).filter(x => !!x)
                var values2 = items.map(x => x[2]).filter(x => !!x)
                return {
                    labels,
                    datasets: [
                        {
                            label: 'valeurs1',
                            backgroundColor: 'rgba(120,120,250,0.5)',
                            data: values
                        },
                        {
                            label: 'valeurs2',
                            backgroundColor: 'rgba(250,120,120,0.5)',
                            data: values2
                        }
                    ]
                }
            }
            return {}
        }
    },
    data () {
        return {
            data: ''
        }
    }
}
</script>
