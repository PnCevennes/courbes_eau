<template>
    <div>
        <label id="fsel_label" for="fsel">Sélectionner un fichier</label>
        <input id="fsel" type="file" @change="open($event)" />
    </div>
</template>
<script>
export default {
    name: 'file-select',
    props: ['value'],
    data () {
        return {
            encoding: 'latin1',
            separator: ';',
            eol: 0,
            data: '',
            file: null
        }
    },
    methods: {
        open (evt) {
            this.file = evt.target.files[0]
            this.read()
        },
        read () {
            var reader = new FileReader()
            reader.onload = this.loaded
            reader.readAsText(this.file, this.encoding)
        },
        loaded (evt) {
            this.data = evt.target.result
            this.parse()
        },
        parse () {
            var EOL = ['\n', '\r\n', '\r']
            var lines = this.data.split(EOL[this.eol])
            var result = [[
                'ID',
                'Date',
                'R',
                'Tracer 1',
                'Tracer 2',
                'Tracer 3',
                'Turbidity',
                'Baseline',
                'Battery V',
                'T',
                'Conductiv'
            ]]
            lines.slice(3).forEach(ln => {
                result.push(ln.trim().split(/  */))
            })
            this.$emit('input', result)
        }
    }
}
</script>
