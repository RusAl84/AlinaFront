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
          <q-btn color="primary" label="Выполнить анализ" @click="onProc" />
        </q-card-section>
      </div>
    </div>
    <div class="big-card">
      <q-card>
        <div class="box">
          <h6>Spa test</h6>
          <img :src="ast" />
        </div>
      </q-card>
      <q-card>
        <div class="box">
          <h6>RS test</h6>
          <img :src="ars" />
        </div>
      </q-card>
      <q-card>
        <div class="box">
          <h6>Количество данных в изображении (байт) (С)</h6>
          <img :src="asz" />
        </div>
      </q-card>
      <q-card>
        <div class="box">
          <h6>Количество данных (байт) (размер сообщения)</h6>
          <img :src="amsz" />
        </div>
      </q-card>
      <q-card>
        <div class="box">
          <h6>Разность между исходным изобр и изобр со стего</h6>
          <img :src="adif" />
        </div>
      </q-card>
      <q-card>
        <div class="box">
          <h6>Коэффициент сжатия стего контейнера</h6>
          <img :src="acomp" />
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
      in_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      out_img_path: ref("http://127.0.0.1:5000/uploads/default.png"),
      in_param: ref(""),
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
    async onProc() {
      console.log("response.data");
      const response = await axios({
        method: "POST",
        url: "http://localhost:5000/stego_reseach",
        data: {
          text: this.input_text,
        },
      });
      console.log("response.data=======================");
      console.log(response.data);
      this.out_img_path = "http://127.0.0.1:5000/uploads/default.png";
      this.out_img_path = response.data.out_img_path;
      this.out_param = response.data.out_param;
      this.ast = response.data.ast;
      this.ars = response.data.ars;
      this.asz = response.data.asz;
      this.amsz = response.data.amsz;
      this.adif = response.data.adif;
      this.acomp = response.data.acomp;
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
