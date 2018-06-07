<template>
  <pre v-html="jsonSyntaxHighlight(json)"></pre>
</template>

<script>
  export default {
    name: 'json-displayer',
    props: {
      json: {
        type: Object,
        default: {}
      }
    },
    methods: {
      jsonSyntaxHighlight(json) {
        if (JSON.stringify(json) === '{}') return ''

        if (typeof json !== 'string') {
          json = JSON.stringify(json, undefined, 2)
        }
        json = json.replace(/&/g, '&').replace(/</g, '<').replace(/>/g, '>')
        /* eslint-disable no-useless-escape */
        const regExp = /("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g
        return json.replace(regExp, (match) => {
          let cls = 'number'
          if (/^"/.test(match)) {
            if (/:$/.test(match)) {
              cls = 'key'
            } else {
              cls = 'string'
            }
          } else if (/true|false/.test(match)) {
            cls = 'boolean'
          } else if (/null/.test(match)) {
            cls = 'null'
          }
          return `<span class="${cls}">${match}</span>`
        })
      }
    }
  }
</script>

<style>
  .string {
    color: green;
  }

  .number {
    color: darkorange;
  }

  .boolean {
    color: blue;
  }

  .null {
    color: magenta;
  }

  .key {
    color: red;
  }
</style>
