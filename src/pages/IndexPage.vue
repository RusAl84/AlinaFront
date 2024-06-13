<template>
  <q-page>
    <div class="box">
      <q-card class="big-card">
        <div class="q-pa-md row items-start q-gutter-md">
          <q-card class="my-card" style="min-width: 450px">
            <img
              :src="in_img_path"
              style="max-width: 430px; max-height: 280px"
            />

            <q-card-section>
              <div class="text-h6">{{ in_param }}</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              {{ in_img_path }}
            </q-card-section>
          </q-card>
          <q-card class="my-card" style="min-width: 450px">
            <img
              :src="out_img_path"
              style="max-width: 430px; max-height: 280px"
            />

            <q-card-section>
              <div class="text-h6">{{ out_param }}</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              {{ out_img_path }}
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
                  <q-uploader
                    :url="hostae_uploadimg"
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
                <q-card-section>
                  <q-btn
                    color="primary"
                    label="Внедрить стего контейнер"
                    @click="onProc"
                  />
                </q-card-section>
              </div>
            </div>
          </q-card>
        </div>
      </q-card>
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
      input_text: ref(
        "Завязался разговор. Готовность белокурого молодого человека в швейцарском плаще отвечать на все вопросы своего черномазого соседа была удивительная и без всякого подозрения совершенной небрежности, неуместности и праздности иных вопросов. Отвечая, он объявил, между прочим, что действительно долго не был в России, с лишком четыре года, что отправлен был за границу по болезни, по какой-то странной нервной болезни, вроде падучей или виттовой пляски, каких-то дрожаний и судорог. Слушая его, черномазый несколько раз усмехался; особенно засмеялся он, когда на вопрос: «Что же, вылечили?» — белокурый отвечал, что «нет, не вылечили»."
      ),
      in_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      out_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      in_param: ref(""),
      out_param: ref(""),
      ttype: ref(""),
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
    async onProc() {
      console.log("response.data");
      const response = await axios({
        method: "POST",
        url: "http://localhost:5000/stego_proc",
        data: {
          text: this.input_text,
        },
      });
      console.log("response.data=======================");
      console.log(response.data);
      this.out_img_path = "http://127.0.0.1:5000/uploads/default.png";
      this.out_img_path = response.data.out_img_path;
      this.out_param = response.data.out_param;
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
