<script setup lang="ts">
// @ts-ignore
import { Decompress } from "7zbson.js"
import { ref } from "vue"

const selectedFile = ref<File | null>(null)
const fileUint8Array = ref<Uint8Array | null>(null)
const resultText = ref<string | null>(null)

const handleFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement
  const file = target.files?.item(0)
  if (file) {
    selectedFile.value = file
    
    var fileReader = new FileReader();
    // ファイルロードが完了したら実行
    fileReader.onload = function webViewerChangeFileReaderOnload(evt) {
      var buffer = evt.target?.result;
      var uint8Array = new Uint8Array(buffer as ArrayBuffer);
      // 生成したUint8Arrayをコンソールに表示
      fileUint8Array.value = uint8Array 
    };
    fileReader.readAsArrayBuffer(file);
  }
}


const handleDecompress = () => {
  console.log("Hi")
  console.log(fileUint8Array.value)
  resultText.value = Decompress(fileUint8Array.value)
}
</script>

<template>
  <div class="container">
    <h1>7zbson test!</h1>
    <input type="file" @change="handleFileChange">
    <button @click="handleDecompress">Decompress</button>
    <textarea :value="resultText || ''"></textarea>
  </div>
</template>
<style>
.container {
  display: flex;
  flex-direction: column;
}
textarea {
  width: 95vw;
  height: 500px;
}
button {
  margin-top: 10px;
  margin-bottom: 10px;
  width: 140px;
  height: 30px;
}
</style>