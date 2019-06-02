<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>ЭКСПЕРТНАЯ СИСТЕМА </span>
        <span class="subheading text-lowercase grey--text">[версия 0.0.1]</span>
      </v-toolbar-title>
    </v-toolbar>

    <v-content>
      <v-container>
        <mn-question
            name="Lorem ipsum dolor sit amet, consectetur adipisicing elit. A accusantium aliquam error illum iste libero nam nisi, odio odit quo reiciendis rem ut! Animi eos fugiat, fugit nam nemo numquam."
            :answer-list="[{id: 1, name: 'answer 1'}]"
        >
        </mn-question>
      </v-container>
    </v-content>

  </v-app>
</template>

<script>

    import MnQuestion from "./components/MnQuestion";
    export default {
        name: 'App',
        components: {
            MnQuestion,
        },
        data () {
            return {
                //
            }
        }
    }
</script>
