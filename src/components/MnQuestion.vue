<template>
    <v-layout column>
        <v-flex>
            <v-card>
                <v-card-text>
                    {{name}}
                </v-card-text>
            </v-card>
        </v-flex>
        <v-flex>
            <v-checkbox
                v-for="(answer, i) in answerList"
                :key="i"
                :label="answer.name"
            ></v-checkbox>
        </v-flex>
    </v-layout>
</template>

<script>
    export default {
        name: "MnQuestion",
        props: {
            name: {
                type: String,
                default: "",
            },
            answerList: {
                type: Array,
                default: () => [],
            },
        },
    }
</script>

<style scoped>

</style>