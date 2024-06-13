<template>
  <q-page class="flex flex-center">
    <div class="q-pa-md row items-start q-gutter-md">
      <div class="big-card">
        <q-card class="my-card" style="min-width: 750px">
          <img
            :src="in_img_path"
            style="max-width: 750px; max-height: 480px; text-align: center"
          />

          <q-card-section>
            <div class="text-h6">{{ in_param }}</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            {{ in_img_path }}
          </q-card-section>
        </q-card>
        <q-card-section>
          <q-uploader
            :url="hostae_uploadimg"
            label="Выберите изображение для анализа .png/*"
            square
            flat
            bordered
            single
            @uploaded="fileUploaded"
            accept=".png, images/*"
            style="min-width: 600px; max-width: 600px"
          />
        </q-card-section>
        <q-card-section>
          <q-uploader
            :url="hostae_uploadimg"
            label="Выберите ключ для дешифровки .dat/*"
            square
            flat
            bordered
            single
            @uploaded="fileUploadedKey"
            accept=".dat, keys/*"
            style="min-width: 600px; max-width: 600px"
          />
        </q-card-section>
        <q-card-section>
          <q-btn
            color="primary"
            label="Извлечь стегоконтейнер"
            @click="onProc"
          />
        </q-card-section>
        <q-card-section>
          <div class="editorclass">
            <q-editor
              v-model="input_text"
              max-height="300px"
              :dense="$q.screen.lt.md"
              :toolbar="[
                [
                  {
                    label: $q.lang.editor.align,
                    icon: $q.iconSet.editor.align,
                    fixedLabel: true,
                    list: 'only-icons',
                    options: ['left', 'center', 'right', 'justify'],
                  },
                  {
                    label: $q.lang.editor.align,
                    icon: $q.iconSet.editor.align,
                    fixedLabel: true,
                    options: ['left', 'center', 'right', 'justify'],
                  },
                ],
                [
                  'bold',
                  'italic',
                  'strike',
                  'underline',
                  'subscript',
                  'superscript',
                ],
                ['token', 'hr', 'link', 'custom_btn'],
                ['print', 'fullscreen'],
                [
                  {
                    label: $q.lang.editor.formatting,
                    icon: $q.iconSet.editor.formatting,
                    list: 'no-icons',
                    options: ['p', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'code'],
                  },
                  {
                    label: $q.lang.editor.fontSize,
                    icon: $q.iconSet.editor.fontSize,
                    fixedLabel: true,
                    fixedIcon: true,
                    list: 'no-icons',
                    options: [
                      'size-1',
                      'size-2',
                      'size-3',
                      'size-4',
                      'size-5',
                      'size-6',
                      'size-7',
                    ],
                  },
                  {
                    label: $q.lang.editor.defaultFont,
                    icon: $q.iconSet.editor.font,
                    fixedIcon: true,
                    list: 'no-icons',
                    options: [
                      'default_font',
                      'arial',
                      'arial_black',
                      'comic_sans',
                      'courier_new',
                      'impact',
                      'lucida_grande',
                      'times_new_roman',
                      'verdana',
                    ],
                  },
                  'removeFormat',
                ],
                ['quote', 'unordered', 'ordered', 'outdent', 'indent'],
                ['undo', 'redo'],
                ['viewsource'],
              ]"
              :fonts="{
                arial: 'Arial',
                arial_black: 'Arial Black',
                comic_sans: 'Comic Sans MS',
                courier_new: 'Courier New',
                impact: 'Impact',
                lucida_grande: 'Lucida Grande',
                times_new_roman: 'Times New Roman',
                verdana: 'Verdana',
              }"
            />
          </div>
        </q-card-section>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import axios from "axios";

export default {
  data() {
    return {
      file: "",
      hostae_uploadimg: "http://localhost:5000/uploadimg",
      hostae_uploadkey: "http://localhost:5000/uploadkey",
      in_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      out_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      in_param: ref(""),
      input_text: ref(""),
      ast: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
      ars: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
      asz: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
      amsz: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
      adif: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
      acomp: ref("http://127.0.0.1:5000/uploads/dafault_plt.png"),
    };
  },
  methods: {
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
    fileUploaded({ files, xhr }) {
      // console.log(xhr.response);
      let data = JSON.parse(xhr.response);
      this.in_img_path = "http://127.0.0.1:5000/uploads/default.png";
      this.in_img_path = data["in_img_path"];
      this.in_param = data["in_param"];
    },
    fileUploadedKey({ files, xhr }) {
      console.log("key uploaded");
    },
    async onProc() {
      console.log("response.data");
      const response = await axios({
        method: "POST",
        url: "http://localhost:5000/stego_decode",
        data: {
          text: this.input_text,
        },
      });
      this.input_text = response.data.decode;
    },
  },
};
</script>
<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
.big-card
  flex-direction: column
  justify-content: center
  align-items: center
  width: 90%
  height: 90%

.box
  justify-content: center
  align-content: center
  align-items: center
  margin: 3%
  text-align: center
  padding-top: 1em
  margin-top: 0.5em
</style>
