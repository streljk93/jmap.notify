<template>
    <v-layout>
        <v-flex pr-2 pb-3>
            <v-btn
                color="success"
                outline
                block
                :disabled="isDisabled"
                @click="$emit('save')">
                <v-icon class="pr-1">save</v-icon>
                сохранить ответ
            </v-btn>
        </v-flex>
        <v-flex px-2 pb-3>
            <v-btn
                color="error"
                outline
                block
                @click="$emit('restart')">
                <v-icon class="pr-1">cancel</v-icon>
                начать сначала
            </v-btn>
        </v-flex>
        <!--<v-flex pl-2 pb-3>-->
            <!--<v-btn outline block>-->
                <!--<v-icon class="pr-1">help</v-icon>-->
                <!--помощь-->
            <!--</v-btn>-->
        <!--</v-flex>-->
    </v-layout>
</template>

<script>
    export default {
        name: "MnQuestionMenu",
        props: {
            isDisabled: {
                type: Boolean,
                default: false,
            }
        }
    }
</script>

<style scoped>

</style>