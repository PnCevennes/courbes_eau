<template>
    <div>
        <table>
            <tr>
                <th :key="index" v-for="(item, index) in headers">{{item}}</th>
            </tr>
            <tr :key="index" v-for="(line, index) in lines">
                <td :key="itemindex" v-for="(item, itemindex) in line">{{item}}</td>
            </tr>
        </table>
    </div>
</template>
<script>
export default {
    name: 'dataTable',
    props: ['value'],
    computed: {
        headers () {
            return this.value.length ? this.value[0].filter(x => x.length) : []
        },
        lines () {
            return this.value.length ? (this.value.slice(1, this.value.length)
                .map(l => l.filter(x => x.length))) : []
        }
    }
}
</script>
