<template>
  <div style="width: 260px">
    <v-form @keyup.enter="submit">
      <v-text-field @keyup.enter="submit" v-model="login" prepend-inner-icon="person" name="login"
                    label="User ID or Email" type="text" autocomplete="username" dense outlined></v-text-field>
      <v-text-field @keyup.enter="submit" v-model="password" prepend-inner-icon="lock" name="password" label="Password"
                    id="password" type="password" autocomplete="current-password" dense outlined></v-text-field>
    </v-form>
    <div class="d-flex justify-space-between align-center">
      <v-btn block
             @click="submit()"
             color="primary"
             :disabled="isLoggingIn"
             :loading='isLoggingIn'>Login
      </v-btn>
    </div>
    <div v-if="loginError" class="text-body-2 error--text mt-2">
      {{ loginError }}
    </div>
    <div class="my-2 text-center">
      <span class="caption"><router-link to="/recover-password">Forgot your password?</router-link></span>
    </div>
  </div>
</template>

<script lang="ts" setup>
import {readLoginError} from '@/store/main/getters';
import {dispatchLogIn} from '@/store/main/actions';
import {commitAddNotification} from '@/store/main/mutations';
import store from '@/store';
import {computed, ref} from "vue";


const isLoggingIn = ref<boolean>(false);
const login = ref<string>('');
const password = ref<string>('');
const loginError = computed(() => {
  return readLoginError(store);
});

async function submit() {
  if (login.value && password.value) {
    try {
      isLoggingIn.value = true;
      await dispatchLogIn(store, {username: login.value, password: password.value});
    } finally {
      isLoggingIn.value = false;
    }
  } else {
    commitAddNotification(store, {
      content: 'Please enter credentials',
      color: 'error',
    });
  }
}
</script>

<style scoped>
</style>
