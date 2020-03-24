<template>
  <div id="file-zone" class="bg-gray-300 p-2">
    <div id="file" class="p-1" contenteditable="true">

    </div>
  </div>
</template>

<script>
export default {
  name: "FileZone",

};
</script>

<style scoped>
#file {
  background-color: #fff;
  border: 1px solid #e4dede;
  min-height: 400px;
}
[contenteditable] {
  outline: 0px solid transparent;
}
</style>
