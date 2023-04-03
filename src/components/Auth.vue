<template>
  <v-container class="fill-height">
    <v-responsive class="d-flex align-center text-center fill-height">
        <v-card>
          <v-form @submit.prevent="login">
            <v-text-field
              v-model="state.email"
              :error-messages="v$.email.$errors.map(e => e.$message)"
              label="E-mail"
              required
              @input="v$.email.$touch"
              @blur="v$.email.$touch"
            ></v-text-field>
            <v-text-field
              v-model="state.password"
              :error-messages="v$.password.$errors.map(e => e.$message)"
              :counter="10"
              label="Password"
              required
              @input="v$.password.$touch"
              @blur="v$.password.$touch"
            ></v-text-field>        
            <v-btn
              class="me-4"
              type="submit"
            >
              submit
            </v-btn>
            <v-btn @click="clear">
              clear
            </v-btn>
          </v-form>
        </v-card>
    </v-responsive>
  </v-container>
</template>

<script>
  import { reactive } from "vue";
  import { useVuelidate } from '@vuelidate/core';
  import { required, email , minLength} from '@vuelidate/validators';
  import { useRouter } from 'vue-router';

  export default{
    setup () {
      const initialState = {
        email: '',
        password: '',
      }

      const state = reactive({
        ...initialState,
      })

      const rules = {
        password: { required, minLength: minLength(6) },
        email: { required, email },
      }
      const v$ = useVuelidate(rules, state)

      function clear () {
        v$.value.$reset()
        for (const [key, value] of Object.entries(initialState)) {
          state[key] = value
        }
      }

      const router = useRouter()

      function login() {
        if (v$.value.$invalid) {
          v$.value.$validate()
          return;
        }
        router.push('/game')
      }
      return { state, clear, login ,v$ }
    },
  }
</script>
