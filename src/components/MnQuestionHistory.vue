<template>
    <v-card class="fill-height">
        <v-card-title>
            <div class="title text-uppercase font-weight-regular" style="text-align: center; width: 100%;">
                История действий
            </div>
        </v-card-title>
        <v-card-text>
            <v-timeline
                align-top
                dense
            >
                <v-timeline-item
                    v-for="(item, i) in value"
                    :key="i"
                    :color="(value.length - 1) === i ? 'success' : 'primary'"
                    small
                >
                    <v-layout pt-3 column>
                        <v-flex class="caption">
                            {{item.date}}
                        </v-flex>
                        <v-flex class="body-1">
                            <span class="body-2">ВОПРОС:</span> {{item.question.name}}
                        </v-flex>
                        <v-flex class="body-1">
                            <span class="body-2">ОТВЕТ:</span> {{item.answer.name}}
                        </v-flex>
                    </v-layout>
                </v-timeline-item>
            </v-timeline>
        </v-card-text>
    </v-card>
</template>

<script>
    export default {
        name: "MnQuestionHistory",
        props: {
            value: {
                type: Array,
                default: () => [],
            }
        }
    }
</script>

<style scoped>

</style>