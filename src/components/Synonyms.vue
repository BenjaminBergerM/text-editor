<template>
  <div class="px-6 pb-4" v-if="word != ''">
    <div class="block text-gray-700 text-x font-bold mb-2">Synonyms of {{ word }}</div>
    <button v-for="(value,index) in synonyms" v-bind:key="index" @click="handleWordClick(value.word)" class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2">{{ value.word }}</button>
  </div>
</template>
<script>
const axios = require('axios')
export default {
  name: 'Synonyms',
  props: [
    'word',
  ],
  data() {
    return {
      synonyms: []
    }
  },
  methods: {
    handleWordClick(word) {
      this.replaceSelectedText(word)
    },
    searchSyno() {
      axios
      .get('https://api.datamuse.com/words?rel_syn=' + this.word)
      .then(response => {
        this.synonyms = response.data
      })
    },
    replaceSelectedText(replacementText) {
      var sel, range;
      if (document.getSelection) {
        let sel = document.getSelection();
        if (sel.rangeCount) {
          range = sel.getRangeAt(0);
          range.deleteContents();
          range.insertNode(document.createTextNode(replacementText));
        }
      }
    }
  },
  watch: {
    word: function (val) {
      this.searchSyno()
    }
  }
}
</script>