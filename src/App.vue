<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>ЭКСПЕРТНАЯ СИСТЕМА </span>
        <span class="subheading text-lowercase grey--text">[версия 0.0.1]</span>
      </v-toolbar-title>
    </v-toolbar>

    <v-content>
      <v-container fill-height>
        <v-layout fill-height>
          <v-flex xs6>
            <mn-question-menu
                :is-disabled="!checkedAnswer"
                @save="handleSave"
                @restart="handleRestart"
                @help="handleHelp"
            ></mn-question-menu>
            <mn-question
                :value="checkedAnswer"
                :type="currentStateObj.type"
                :name="currentStateObj.name"
                :answer-list="currentAnswerList"
                @answer-id="handleGetAnswerId"
            ></mn-question>
          </v-flex>
          <v-flex xs6 ml-4 mt-1>
            <mn-question-history :value="stateHistory"></mn-question-history>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>

    <v-dialog
        v-model="dialog"
        width="500"
    >
      <v-card>
        <v-card-title
          class="title font-weight-regular"
          primary-title>
          БЛОК ОБЪЯСНЕНИЙ
        </v-card-title>

        <v-card-text>
            {{currentHelp}}
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="primary"
              flat
              @click="dialog = false"
          >
            ЗАКРЫТЬ
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-app>
</template>

<script>

    import moment from "moment";
    import MnQuestion from "./components/MnQuestion";
    import MnQuestionMenu from "./components/MnQuestionMenu";
    import MnQuestionHistory from "./components/MnQuestionHistory";
    export default {
        name: 'App',
        components: {
            MnQuestionHistory,
            MnQuestionMenu,
            MnQuestion,
        },
        data () {
            const stateType = {
                QUESTION: 1,
                RESULT: 2,
            };

            return {
                currentState: 0,
                checkedAnswer: null,
                state: [{
                    id: 0,
                    name: "У вас есть запланированная встреча?",
                    type: stateType.QUESTION,
                }, {
                    id: 1,
                    name: "Совпадает ли время оповещения запланированной встречи с временем на телефоне?",
                    type: stateType.QUESTION,
                }, {
                    id: 3,
                    name: "Есть ли рядом автобусные остановки в радиусе 500 м?",
                    type: stateType.QUESTION,
                }, {
                    id: 4,
                    name: "Есть ли рядом автобусные остановки в радиусе 1000 м?",
                    type: stateType.QUESTION,
                }, {
                    id: 5,
                    name: "На найденных автобусных остановках есть автобусы?",
                    type: stateType.QUESTION,
                }, {
                    id: 7,
                    name: "Какая дистанция ближайшего автобуса идущего по вашему маршруту до остановки?",
                    type: stateType.QUESTION,
                }, {
                    id: 9,
                    name: "Какая дистанция от вас и до остановки с ближайшим автобусом?",
                    type: stateType.QUESTION,
                }, {
                    id: 10,
                    name: "Какая дистанция от вас и до остановки с ближайшим автобусом?",
                    type: stateType.QUESTION,
                }, {
                    id: 2,
                    name: "Поиск автотранспорта не будет выполнен",
                    type: stateType.RESULT,
                }, {
                    id: 6,
                    name: "К сожалению рядом с вами ближайших остановок не найдено",
                    type: stateType.RESULT,
                }, {
                    id: 8,
                    name: "К сожалению на ближайшей остановке автобусы не ходят",
                    type: stateType.RESULT,
                }, {
                    id: 11,
                    name: "Автобус будет находится в пути еще 20 + мин, мы вас оповестим когда вам можно будет выходить к остановке",
                    type: stateType.RESULT,
                }, {
                    id: 12,
                    name: "Вы успеваете на свой маршрут, можете выходить и не торопиться",
                    type: stateType.RESULT,
                }, {
                    id: 13,
                    name: "Если вы выйдете прямо сейчас и будите идти быстрым шагом, то вы успеете",
                    type: stateType.RESULT,
                }, {
                    id: 14,
                    name: "Вы не успеваете на ближайший маршрут, дождитесь другого автобуса",
                    type: stateType.RESULT,
                }],
                answer: [{
                    id: 1,
                    name: "Да, у меня есть запланированная встреча.",
                }, {
                    id: 2,
                    name: "У меня нет запланированных встреч.",
                }, {
                    id: 3,
                    name: "Текущее время совпадает с запланированным.",
                }, {
                    id: 4,
                    name: "Текущее время не совпадает с запланированным.",
                }, {
                    id: 5,
                    name: "В радиусе 500 метров не найдены автобусные остановки.",
                }, {
                    id: 6,
                    name: "В радиусе 500 метров находятся автобусные остановки.",
                }, {
                    id: 7,
                    name: "В радиусе 1000 метров находятся автобусные остановки.",
                }, {
                    id: 8,
                    name: "В радиусе 1000 метров не найдены автобусные остановки.",
                }, {
                    id: 9,
                    name: "На найденных остановках есть действующие автобусы.",
                }, {
                    id: 10,
                    name: "На найденных остановка нет действующих автобусов.",
                }, {
                    id: 11,
                    name: "Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> = 4 км и <= 7 км.",
                }, {
                    id: 12,
                    name: "Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> = 7 км и <= 12 км.",
                }, {
                    id: 13,
                    name: "Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> 12 км.",
                }, {
                    id: 14,
                    name: "Дистанция от пользователя и до остановки составляет <400 м.",
                }, {
                    id: 15,
                    name: "Дистанция от пользователя и до остановки составляет> = 400 м и <= 700 м.",
                }, {
                    id: 16,
                    name: "Дистанция от пользователя и до остановки составляет> 700 м.",
                }, {
                    id: 17,
                    name: "Дистанция от пользователя и до остановки составляет <900 м.",
                }, {
                    id: 18,
                    name: "Дистанция от пользователя и до остановки составляет> 900 м.",
                }],
                stateAnswer: [{
                    id: 1,
                    state: 0,
                    answer: 1,
                }, {
                    id: 2,
                    state: 0,
                    answer: 2,
                }, {
                    id: 3,
                    state: 1,
                    answer: 3,
                }, {
                    id: 4,
                    state: 1,
                    answer: 4,
                }, {
                    id: 5,
                    state: 3,
                    answer: 5,
                }, {
                    id: 6,
                    state: 3,
                    answer: 6,
                }, {
                    id: 7,
                    state: 4,
                    answer: 7,
                }, {
                    id: 8,
                    state: 4,
                    answer: 8,
                }, {
                    id: 9,
                    state: 5,
                    answer: 9,
                }, {
                    id: 10,
                    state: 5,
                    answer: 10,
                }, {
                    id: 11,
                    state: 7,
                    answer: 11,
                }, {
                    id: 12,
                    state: 7,
                    answer: 12,
                }, {
                    id: 13,
                    state: 7,
                    answer: 13,
                }, {
                    id: 14,
                    state: 9,
                    answer: 14,
                }, {
                    id: 15,
                    state: 9,
                    answer: 15,
                }, {
                    id: 16,
                    state: 9,
                    answer: 16,
                }, {
                    id: 17,
                    state: 10,
                    answer: 17,
                }, {
                    id: 18,
                    state: 10,
                    answer: 18,
                }],
                solver: [{
                    start: 0,
                    end: 1,
                    isEnd: 0,
                    answer: 1,
                }, {
                    start: 0,
                    end: 2,
                    isEnd: 1,
                    answer: 2,
                }, {
                    start: 1,
                    end: 3,
                    isEnd: 0,
                    answer: 3,
                }, {
                    start: 1,
                    end: 2,
                    isEnd: 1,
                    answer: 4,
                }, {
                    start: 3,
                    end: 4,
                    isEnd: 0,
                    answer: 5,
                }, {
                    start: 3,
                    end: 5,
                    isEnd: 0,
                    answer: 6,
                }, {
                    start: 4,
                    end: 5,
                    isEnd: 0,
                    answer: 7,
                }, {
                    start: 4,
                    end: 6,
                    isEnd: 1,
                    answer: 8,
                }, {
                    start: 5,
                    end: 7,
                    isEnd: 0,
                    answer: 9,
                }, {
                    start: 5,
                    end: 8,
                    isEnd: 1,
                    answer: 10,
                }, {
                    start: 7,
                    end: 9,
                    isEnd: 0,
                    answer: 11,
                }, {
                    start: 7,
                    end: 10,
                    isEnd: 0,
                    answer: 12,
                }, {
                    start: 7,
                    end: 11,
                    isEnd: 1,
                    answer: 13,
                }, {
                    start: 9,
                    end: 12,
                    isEnd: 1,
                    answer: 14,
                }, {
                    start: 9,
                    end: 13,
                    isEnd: 1,
                    answer: 15,
                }, {
                    start: 9,
                    end: 14,
                    isEnd: 1,
                    answer: 16,
                }, {
                    start: 10,
                    end: 12,
                    isEnd: 1,
                    answer: 17,
                }, {
                    start: 10,
                    end: 13,
                    isEnd: 1,
                    answer: 18,
                }],
                help: [{
                    id: 1,
                    state: 0,
                    description: "Добавляли ли вы заранее маршрут в систему с датой и временем оповещения?",
                }, {
                    id: 2,
                    state: 1,
                    description: "Вы выбрали вариант «Да, у меня есть запланированная встреча.» поэтому нужно проверить текущее время (системное время на телефоне) с запланированным.",
                }, {
                    id: 3,
                    state: 2,
                    description: "Если у вас нет запланированного маршрута или текущее время не совпадает с запланированным, то не будет выполняться скрипт и поиск автотранспорта не будет выполнен.",
                }, {
                    id: 4,
                    state: 3,
                    description: "Вы выбрали вариант «Текущее время совпадает с запланированным» поэтому нужно проверить все остановки которые находятся рядом с вами в радиусе 500 метров. Если будет найдены остановки, то система их автоматически выберет",
                }, {
                    id: 5,
                    state: 4,
                    description: "Вы выбрали вариант «В радиусе 500 метров не найдены автобусные остановки» поэтому нужно увеличить радиус поиска в 2 раза.",
                }, {
                    id: 6,
                    state: 5,
                    description: "Были найдены остановки рядом с вами, поэтому нужно проверить их на действующие автобусы. Возможно есть автобусы, которые больше не ходят по этой остановки",
                }, {
                    id: 7,
                    state: 6,
                    description: "Вы выбрали вариант «В радиусе 1000 метров не найдены автобусные остановки» поэтому скрипт остановился и дальнейший поиск маршрута не будет выполнен.",
                }, {
                    id: 8,
                    state: 7,
                    description: "Вы выбрали вариант «На найденных остановках есть действующие автобусы» поэтому нужно определить расстояние ближайшего автобуса (по gps-трекеру) до остановки, которая была выбрана в результате поиска остановок.",
                }, {
                    id: 9,
                    state: 8,
                    description: "Вы выбрали вариант «На найденных остановка нет действующих автобусов» поэтому скрипт остановился и дальнейший поиск маршрута не будет выполнен.",
                }, {
                    id: 10,
                    state: 9,
                    description: "Вы выбрали вариант «Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> = 4 км и <= 7 км». Теперь нужно посчитать вашу дистанцию до остановки, к которой будет подходить автобус и проверим успеваете ли вы дойти до ближайшей остановки вовремя или нужно будет ждать следующий автобус.",
                }, {
                    id: 11,
                    state: 10,
                    description: "Вы выбрали вариант «Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> = 7 км и <= 12 км». Теперь нужно посчитать вашу дистанцию до остановки, к которой будет подходить автобус и проверим успеваете ли вы дойти до ближайшей остановки вовремя или нужно будет ждать следующий автобус.",
                }, {
                    id: 12,
                    state: 11,
                    description: "Вы выбрали вариант «Дистанция ближайшего автобуса, идущего по нужному маршруту, составляет> 12 км» поэтому система будет заново проходить по циклу и проверять все автобусы, которые попадут в зону поиска. Ожидайте, ваш автобус еще в пути.",
                }, {
                    id: 13,
                    state: 12,
                    description: "Вы выбрали вариант «Дистанция от пользователя и до остановки составляет <400 м.» или «Дистанция от пользователя и до остановки составляет <900 м.», это составляет 4 или 9 минут ходьбы до остановки соответственно. Сам же автобус подъедет к остановке через 10 – 15 мин, поэтому вы успеваете.",
                }, {
                    id: 14,
                    state: 13,
                    description: "Вы выбрали вариант «Дистанция от пользователя и до остановки составляет> = 400 м и <= 700 м» или «Дистанция от пользователя и до остановки составляет> 900 м». это составляет ~6 или ~12 минут ходьбы до остановки соответственно. Сам же автобус подъедет к остановке через 10 – 15 мин, поэтому вам нужно будет поторопиться.",
                }, {
                    id: 15,
                    state: 14,
                    description: "Вы выбрали вариант «Дистанция от пользователя и до остановки составляет> 700 м.» когда как автобус уже подходит к остановке. Поэтому вы не успеваете на свой маршрут, вам стоит дождаться другой.",
                }],
                stateHistory: [],
                dialog: false,
            };
        },

        computed: {
            currentStateObj() {
                return this.state.find(v => v.id === this.currentState);
            },
            currentAnswerList() {
                const stateAnswer = this.stateAnswer.filter(v => v.state === this.currentState);

                return this.answer.filter(answer => {
                    return Boolean(stateAnswer.find(v => v.answer === answer.id));
                });
            },
            currentHelp() {
                const help = this.help.find(v => v.state === this.currentState) || {};
                
                return help.description || "";
            },
        },
        methods: {
            handleSave() {
                const next = this.solver.find(v => {
                    return v.start === this.currentState && v.answer === this.checkedAnswer;
                });
                this.addToHistory({
                    question: this.currentStateObj,
                    answer: this.answer.find(v => v.id === this.checkedAnswer),
                });
                this.currentState = next.end;
                this.checkedAnswer = null;
            },
            handleRestart() {
                this.currentState = 0;
                this.checkedAnswer = null;
                this.stateHistory = [];
            },
            handleHelp() {
                this.dialog = true;
            },
            handleGetAnswerId(id) {
                this.checkedAnswer = id;
            },
            addToHistory(item) {
                item.date = moment().format('YYYY-MM-DD HH:mm:ss');
                this.stateHistory.push(item);
            },
        }
    }
</script>
