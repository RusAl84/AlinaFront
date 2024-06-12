<template>
  <q-page>
    <div class="box">
      <q-card class="big-card">
        <div class="q-pa-md row items-start q-gutter-md">
          <q-card class="my-card" style="min-width: 450px">
            <img
              src="~assets/a1.png"
              style="max-width: 430px; max-height: 280px"
            />

            <q-card-section>
              <div class="text-h6">Our Changing Planet</div>
              <div class="text-subtitle2">by John Doe</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              {{ lorem }}
            </q-card-section>
          </q-card>
          <q-card class="my-card" style="min-width: 450px">
            <img src="https://cdn.quasar.dev/img/mountains.jpg" />

            <q-card-section>
              <div class="text-h6">Our Changing Planet</div>
              <div class="text-subtitle2">by John Doe</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              {{ lorem }}
            </q-card-section>
          </q-card>

          <q-card class="my-card" style="min-width: 900px">
            <div class="flexrow">
              <div>
                <q-card-section>
                  <div class="text-h6">Введите текст для отправки:</div>
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
                            options: [
                              'p',
                              'h1',
                              'h2',
                              'h3',
                              'h4',
                              'h5',
                              'h6',
                              'code',
                            ],
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
                <q-card-section>
                  <q-btn color="primary" label="Поиск" @click="onFind_data" />
                </q-card-section>
              </div>
            </div>
            <q-card-section>
              <q-uploader
                :url="hostae_uploadae"
                label="Выберите изображение для внедрения стегоконтейнера .png/*"
                square
                flat
                bordered
                single
                @uploaded="fileUploaded"
                accept=".png, images/*"
                style="min-width: 600px; max-width: 600px"
              />
            </q-card-section>
          </q-card>
        </div>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import axios from "axios";
import * as cfg from "../config.js";

export default {
  data() {
    return {
      file: "",
      hostae: cfg.hostae,
      hostae_uploadae: cfg.hostae_uploadae,
      chatmessages: "",
      proc_text: ref(""),
      ttype: ref(""),
      resp_text: ref(""),
      resp_data: ref(""),
      all_data: ref([{}]),
      ae_data: ref([{}]),
      filename: ref(""),
      options: ["RAKE", "YAKE", "BERT"],
    };
  },
  methods: {
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
    fileUploaded({ files, xhr }) {
      let data = JSON.parse(xhr.response);
      this.chatmessages = data["text"];
      this.filename = data["filename"];
    },
    async onProc() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "get_pattern",
        data: {
          text: this.proc_text,
        },
      });
      console.log(response);
      this.resp_data = response.data;
      this.resp_text = response.data.print_text;
    },
    async onProcAdd() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "get_pattern_add",
        data: this.resp_data,
      });
      this.all_data = response.data;
      console.log("this.resp_data");
      console.log(this.all_data);
    },
    async onLoadDB() {
      const response = await axios({
        method: "get",
        url: cfg.hostae + "load_db",
        data: this.resp_data,
      });
      this.all_data = response.data;

      console.log(this.all_data);
    },
    async onClearDB() {
      const response = await axios({
        method: "get",
        url: cfg.hostae + "clear_db",
      });
      console.log(response);
    },
    async onFindCL() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "findae",
        data: {
          filename: this.filename,
          type: this.ttype,
        },
        headers: {
          "Content-Type": "application/json",
        },
      });
      // console.log(response);
      this.ae_data = response.data;
      console.log("ae_data");
      console.log(this.ae_data);
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
  display: flex
  flex-flow: row nowrap
  justify-content: center
  align-content: center
  align-items: center
  margin: 3%
</style>
