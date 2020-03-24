<template>
  <div class="w-full rounded overflow-hidden shadow-lg mt-4">
    <div class="px-6 py-4">
      <div class="font-bold text-xl mb-2">Text Editor</div>
      <ControlPanel @findSynonyms="seart => word = seart"/>
      <FileZone/>
    </div>
    <Synonyms :word="word"/>
  </div>
</template>

<script>
import ControlPanel from './ControlPanel'
import FileZone from './FileZone'
import Synonyms from './Synonyms'

export default {
  name: 'TextEditor',
  components: {
    ControlPanel,
    FileZone,
    Synonyms
  },
  data() {
    return {
      word: ''
    }
  },
}
</script>
