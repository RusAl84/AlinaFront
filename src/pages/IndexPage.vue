<template>
  <q-page>
    <div class="box">
      <q-card class="big-card">
        <div class="flex flex-center">
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
          </div>

          <q-card class="my-card" style="min-width: 900px">
            <div class="flexrow">
              <div>
                <q-card-section>
                  <div class="text-h6">Поиск целевой информации:</div>
                </q-card-section>
                <q-input
                  class="inputtext"
                  v-model="find_text"
                  type="textarea"
                  autogrow
                  float-label="Введите текст для анализа"
                />
                <q-card-section>
                  <q-btn color="primary" label="Поиск" @click="onFind_data" />
                </q-card-section>
                <q-card-section>
                  <div class="text-h6">
                    Полученная сигнатура (сжатый портрет):
                  </div>
                </q-card-section>
                <q-input
                  class="inputtext"
                  v-model="sig_text"
                  type="textarea"
                  readonly
                  autogrow
                />
                <q-card-section>
                  <q-btn
                    color="primary"
                    label="Получить сигнатуры (сжатый портрет)"
                    @click="onGet_Sig"
                  />
                </q-card-section>
              </div>
            </div>

            ddd
          </q-card>
        </div>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
});
</script>
<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
.big-card
  flex-direction: column
  justify-content: center
  flex-center
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
