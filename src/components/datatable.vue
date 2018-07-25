<template>
    <div>
        <table class="datatable">
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
<style>
table {
    border: 1px solid black;
    border-spacing: 0;
    width: 100%;
}
tr, td, th {
    border: 1px solid #999999;
    padding: 2px 10px;
}

th {
    background-color: #AAAAFF;
    padding: 5px 10px;
}

tr:nth-of-type(5n-4) {
    background-color: #EEEEEE;
}
</style>
