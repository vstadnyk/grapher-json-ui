# Grapher-JSON-UI

Simple Vue component with basic GUI to print and/or edit JSON data.

[Demo](https://vstadnyk.github.io/grapher-json-ui-example/)

## Install

```bash
npm install grapher-json-ui --save
```

```bash
yarn add grapher-json-ui
```

### Usage (default)

```vue
<template>
    <grapher v-model="myData" />
</template>

<script>
import 'grapher-json-ui/dist/index.css'

import Grapher from 'grapher-json-ui'

export default {
    components: { Grapher },
    data: () => ({
        myData: {
            test: 'Hello world'
        }
    })
}
</script>
```

### Usage (edit mode)

```vue
<template>
    <grapher v-model="myData" :editable="true" />
</template>

<script>
import 'grapher-json-ui/dist/index.css'

import Grapher from 'grapher-json-ui'

export default {
    components: { Grapher },
    data: () => ({
        myData: {
            test: 'Hello world'
        }
    })
}
</script>
```

## Props

```javascript
{
    editable: {
        type: Boolean,
        default: false
    },
    brackets: {
        type: Boolean,
        default: true
    },
    comma: {
        type: Boolean,
        default: true
    }
}
```