<template>
    <div>
        <div>
            <label for="encoding">Encodage du fichier</label>
            <select id="encoding" v-model="encoding" @change="read">
                <option value="utf8">UTF-8</option>
                <option value="utf16">UTF-16</option>
                <option value="latin1">Latin1</option>
            </select>
        </div>
        <div>
            <label for="eol">Identificateur de fin de ligne</label>
            <select id="eol" v-model="eol" @change="parse">
                <option value="0">LF (linux)</option>
                <option value="1">CRLF (Windows)</option>
                <option value="2">CR (MacOS)</option>
            </select>
        </div>
        <div>
            <label for="separator">Séparateur de données</label>
            <input id="separator" type="text" v-model="separator" @input="parse" />
        </div>
        <input type="file" @change="open($event)" />
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
            var result = []
            lines.forEach(ln => {
                result.push(ln.split(this.separator))
            })
            this.$emit('input', result)
        }
    }
}
</script>
