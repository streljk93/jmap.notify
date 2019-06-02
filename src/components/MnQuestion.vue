<template>
    <v-layout column>
        <v-flex>
            <v-card v-if="type === 1">
                <v-card-text>
                    {{name}}
                </v-card-text>
            </v-card>
            <v-card v-else-if="type === 2" color="success">
                <v-card-text class="white--text">
                    {{name}}
                </v-card-text>
            </v-card>
        </v-flex>
        <v-flex>
            <v-radio-group v-model="checkedAnswer" :mandatory="false">
                <v-radio
                    v-for="(answer, i) in answerList"
                    :key="i"
                    :value="answer.id"
                    :label="answer.name"
                    color="success"
                    hide-detail
                    @change="$emit('answer-id', answer.id)"
                ></v-radio>
            </v-radio-group>
        </v-flex>
    </v-layout>
</template>

<script>
    export default {
        name: "MnQuestion",
        props: {
            value: {
                type: Number,
                default: null,
            },
            name: {
                type: String,
                default: "",
            },
            type: {
                type: Number,
                default: 1,
            },
            answerList: {
                type: Array,
                default: () => [],
            },
        },
        data() {
            return {
                checkedAnswer: null,
            };
        },
        watch: {
            value(val) {
                if (val === null) {
                    this.checkedAnswer = null;
                }
            }
        }
    }
</script>

<style scoped>

</style>