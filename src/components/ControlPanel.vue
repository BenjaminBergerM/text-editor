<template>
  <div class="inline-flex">
    <button class="text-gray-800 font-bold py-2 px-4 rounded-tl" v-bind:class="{  'bg-gray-400 hover:bg-gray-300' : bold.selected, 'bg-gray-300 hover:bg-gray-400' : !bold.selected  }" type="button" @click="handleFormatDoc('bold')"><i class="fas fa-bold"></i></button>
    <button class="text-gray-800 font-bold py-2 px-4" v-bind:class="{  'bg-gray-400 hover:bg-gray-300' : italic.selected, 'bg-gray-300 hover:bg-gray-400' : !italic.selected  }" type="button" @click="handleFormatDoc('italic')"><i class="fas fa-italic"></i></button>
    <button class="text-gray-800 font-bold py-2 px-4" v-bind:class="{  'bg-gray-400 hover:bg-gray-300' : underline.selected, 'bg-gray-300 hover:bg-gray-400' : !underline.selected  }" type="button" @click="handleFormatDoc('underline')"><i class="fas fa-underline"></i></button>
    <button class="text-gray-800 font-bold py-2 px-4" v-bind:class="{  'bg-gray-400 hover:bg-gray-300' : strikeThrough.selected, 'bg-gray-300 hover:bg-gray-400' : !strikeThrough.selected  }" type="button" @click="handleFormatDoc('strikeThrough')"><i class="fas fa-strikethrough"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('insertHorizontalRule')"><i class="fas fa-minus"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('justifyLeft')"><i class="fas fa-align-left"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('justifyCenter')"><i class="fas fa-align-center"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('justifyRight')"><i class="fas fa-align-right"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('removeFormat')"><i class="fas fa-remove-format"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('copy')"><i class="fas fa-copy"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('cut')"><i class="fas fa-cut"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleFormatDoc('removeFormat')"><i class="fas fa-remove-format"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4" type="button" @click="handleOpenLinkModal()"><i class="fas fa-link"></i></button>
    <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-tr" type="button" @click="handleFindSynonyms()"><i class="fas fa-equals"></i></button>
    <Modal :showing="link.modal.visible" @close="link.modal.visible = false">
      <div class="w-full">
        <div class="mb-4">
          <label class="block text-gray-700 text-x font-bold mb-2" for="username">
            Enter the url
          </label>
          <input class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="username" type="text" placeholder="http://google.com" v-model="link.url">
        </div>
        <div class="flex items-center justify-between">
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button" @click="handleAddLink()">
            Add
          </button>
        </div>
      </div>
    </Modal>
  </div>
</template>

<script>
import Modal from './Modal'

export default {
  name: "ControlPanel",
  components: {
    Modal
  },
  data() {
    return {
      link: {
        modal: {
          visible: false,
        },
        selection: 0,
        url: '',
      },
      bold: {
        selected: false
      },
      italic: {
        selected: false
      },
      underline: {
        selected: false
      },
      strikeThrough: {
        selected: false
      },
      fontSize: {
        modal: {
          visible: false
        },
        size: ''
      }
    }
  },
  methods: {
    handleFormatDoc(command, args) {
      document.execCommand(command, true, args);
    },
    handleOpenLinkModal() {
      this.link.modal.visible = true
      this.link.selection = this.saveSelection()
    },
    handleAddLink() {
      this.restoreSelection(this.link.selection)
      this.handleFormatDoc('createLink', this.link.url);
      this.link.modal.visible = false
    },
    handleFindSynonyms() {
      let word = window.getSelection().toString()
      this.$emit('findSynonyms', word)
    },
    saveSelection() {
      if (document.getSelection) {
        let sel = document.getSelection();
        if (sel.getRangeAt && sel.rangeCount) {
          return sel.getRangeAt(0);
        }
      }
      return null;
    },
    restoreSelection(range) {
      if (range) {
        if (document.getSelection) {
          let sel = document.getSelection();
          sel.removeAllRanges();
          sel.addRange(range);
        }
      }
    },
    selectionIsBold() {      
      if (document.queryCommandState) {
          this.bold.selected = document.queryCommandState("bold");
          this.italic.selected = document.queryCommandState("italic");
          this.underline.selected = document.queryCommandState("underline");
          this.strikeThrough.selected = document.queryCommandState("strikeThrough");
      }
    }
  },
  mounted() {
    document.execCommand("defaultParagraphSeparator", false, "p");
    document.addEventListener('selectionchange', () => {
      this.selectionIsBold()
    });

  }
};
</script>
<style scoped>
input[type="button"]{
   outline:none;
}
input[type="button"]::-moz-focus-inner {
   border: 0;
}
</style>
