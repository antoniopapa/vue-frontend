<template>
  <main class="form-signin">
    <form @submit.prevent="submit">
      <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

      <input v-model="form.email" type="email" class="form-control" placeholder="Email" required>

      <input v-model="form.password" type="password" class="form-control" placeholder="Password" required>

      <button class="w-100 btn btn-lg btn-primary" type="submit">Submit</button>
    </form>
  </main>
</template>

<script>
import {reactive} from 'vue';
import axios from "axios";
import {useRouter} from "vue-router";

export default {
  name: "Login",
  setup() {
    const form = reactive({
      email: '',
      password: ''
    });
    const router = useRouter();

    const submit = async () => {
      await axios.post('login', {
        email: form.email,
        password: form.password
      });

      await router.push('/');
    }

    return {
      form,
      submit
    }
  }
}
</script>

<style scoped>
html,
body {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.form-signin .checkbox {
  font-weight: 400;
}

.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}

.form-signin .form-control:focus {
  z-index: 2;
}

.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
