<template>
  <img alt="Vue logo" src="./assets/logo.png"/>
  <HelloWorld msg="Hello Vue 3.0 + Vite"/>
  <p>data : {{ d }}</p>
  <p>type : {{ type }}</p>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import {onMounted, ref} from "vue";

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    const d = ref()
    const type = ref()


    onMounted(async () => {
      try {
        const reader = new NDEFReader()
        await reader.scan()

        reader.addEventListener('error', e => {
          console.error(e)
        })

        reader.addEventListener('reading', ({serialNumber, message}) => {
          const record = message.records[0]
          const {data, recordType} = record
          d.value = data
          type.value = recordType
        })
      } catch (e) {
        console.error(e)
      }
    })

    return {
      d,
      type
    }

  }
}
</script>
