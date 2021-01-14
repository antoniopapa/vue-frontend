<template>
  <form @submit.prevent="submit">
    <div class="mb-3">
      <label>First Name</label>
      <input v-model="data.first_name" class="form-control" name="first_name">
    </div>
    <div class="mb-3">
      <label>Last Name</label>
      <input v-model="data.last_name" class="form-control" name="last_name">
    </div>
    <div class="mb-3">
      <label>Email</label>
      <input v-model="data.email" class="form-control" name="email">
    </div>

    <div class="mb-3">
      <label>Role</label>
      <select v-model="data.role_id" name="role_id" class="form-control">
        <option v-for="role of roles" :key="role.id" :value="role.id">
          {{ role.name }}
        </option>
      </select>
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import {reactive, ref, onMounted} from "vue";
import axios from 'axios';
import {useRoute, useRouter} from "vue-router";

export default {
  name: "UserEdit",
  setup() {
    const data = reactive({
      first_name: '',
      last_name: '',
      email: '',
      role_id: ''
    });
    const roles = ref([]);
    const router = useRouter();
    const route = useRoute();

    onMounted(async () => {
      const rolesResponse = await axios.get('roles');

      roles.value = rolesResponse.data;

      const response = await axios.get(`users/${route.params.id}`);

      data.first_name = response.data.first_name;
      data.last_name = response.data.last_name;
      data.email = response.data.email;
      data.role_id = response.data.role.id;
    });

    const submit = async () => {
      await axios.put(`users/${route.params.id}`, data);

      await router.push('/users');
    }

    return {
      data,
      roles,
      submit
    }
  }
}
</script>
