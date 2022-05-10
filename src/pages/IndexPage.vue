<template>
  <q-page class="row items-center justify-evenly container">
    <div class="flex flex-center">
      <div class="q-pa-xl" style="min-width: 400px">
        <h1 class="title">Программа обучения</h1>
        <q-form @submit.prevent="onSubmit" novalidate>
          <div v-show="step === 1" class="step q-gutter-md">
            <q-select
              outlined
              filled
              v-model="direction"
              :options="directionOfStudy"
              label="Направление обучения"
            />

            <q-select
              outlined
              filled
              v-model="formOfStudy"
              :options="formOfTeaching"
              label="Форма обучения"
            />

            <div>
              <q-btn @click="nextStep" label="Следующий шаг" color="primary" />
            </div>
          </div>

          <div v-show="step === 2" class="step q-gutter-md">
            <q-input filled v-model="date" mask="date" :rules="['date']">
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                  <q-popup-proxy
                    ref="qDateProxy"
                    cover
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date v-model="date" :locale="currentLocale">
                      <div class="row items-center justify-end">
                        <q-btn
                          v-close-popup
                          label="Закрыть"
                          color="primary"
                          flat
                        />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </q-icon>
              </template>
            </q-input>

            <q-input v-model="text" filled type="textarea" />
            <q-select
              outlined
              filled
              v-model="location"
              :options="locationOfStudy"
              label="Место проведения"
            />
            <div class="flex justify-between">
              <q-btn
                @click="previousStep"
                label="Назад"
                color="primary"
                flat
                class="q-ml-sm"
              />
              <q-btn label="Отправить" type="submit" color="primary" />
            </div>
          </div>
        </q-form>
      </div>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { api } from 'boot/axios';
import { date } from 'quasar';

export default defineComponent({
  name: 'IndexPage',
  components: {},
  data() {
    return {
      step: 1,
      direction: ref(null),
      formOfStudy: ref(null),
      text: ref(''),
      location: ref(null),
      date: date.formatDate(Date.now(), 'YYYY/MM/DD'),
      directionOfStudy: [
        'Секретное делопроизводство',
        'Допускная работа в организации',
        'Архивное дело в организациях атомной отрасли',
      ],
      formOfTeaching: ['очное', 'заочное', 'дистанционное'],
      locationOfStudy: ['Москва', 'Санкт-Петербург', 'Удаленное'],
      currentLocale: {
        days: 'Воскресенье_Понедельник_Вторник_Среда_Четверг_Пятница_Суббота_Воскресенье'.split(
          '_'
        ),
        daysShort: 'Вс_Пн_Вт_Ср_Чт_Пт_Сб'.split('_'),
        months:
          'Январь_Февраль_Март_Апрель_Май_Июнь_Июль_Август_Сентябрь_Октябрь_Ноябрь_Декабрь'.split(
            '_'
          ),
        monthsShort: 'Янв_Фев_Мар_Апр_Май_Июн_Июл_Авг_Сен_Окт_Ноя_Дек'.split(
          '_'
        ),
      },
    };
  },
  methods: {
    nextStep() {
      this.step = 2;
    },
    previousStep() {
      this.step = 1;
    },
    onSubmit() {
      api.post('/', {
        direction: this.direction,
        formOfStudy: this.formOfStudy,
        date: this.date,
        text: this.text,
        location: this.location,
      });
      this.direction = null;
      this.formOfStudy = null;
      this.date = date.formatDate(Date.now(), 'YYYY/MM/DD');
      this.text = '';
      this.location = null;
    },
  },
});
</script>

<style>
.container {
  background-image: url('./../assets/bg7.jpg');
  background-size: 100%;
}
</style>
