<script>
import {Line} from 'vue-chartjs'

export default {
    name: 'lineChart',
    extends: Line,
    props: ['data'],
    mounted () {
        this.renderChart(this.data)
    },
    watch: {
        data (nv) {
            this.renderChart(nv)
        }
    }
}
</script>
