<template>
  <form @submit.prevent="submit">
    <div class="mb-3 mt-3 row">
      <label class="col-sm-2 col-form-label">Name</label>
      <div class="col-sm-10">
        <input v-model="formData.name" class="form-control" name="name">
      </div>
    </div>

    <div class="mb-3 row">
      <label class="col-sm-2 col-form-label">Permissions</label>
      <div class="col-sm-10">
        <div class="form-check form-check-inline col-3" v-for="permission in permissionList" :key="permission.id">
          <input class="form-check-input" type="checkbox" :value="permission.id"
                 @change="select(permission.id, $event.target.checked)">
          <label class="form-check-label">{{ permission.name }}</label>
        </div>
      </div>
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import {onMounted, reactive, ref} from 'vue';
import axios from 'axios';
import {useRouter} from "vue-router";

export default {
  name: "RoleCreate",
  setup() {
    const {push} = useRouter();

    const formData = reactive({
      name: '',
      permissions: [] as number[]
    });
    const permissionList = ref([]);

    onMounted(async () => {
      const {data} = await axios.get('permissions');

      permissionList.value = data;
    });

    const select = (id: number, checked: boolean) => {
      if (checked) {
        formData.permissions = [...formData.permissions, id];
        return;
      }

      formData.permissions = formData.permissions.filter(p => p !== id);
    }

    const submit = async () => {
      await axios.post('roles', formData);

      await push('/roles');
    }

    return {
      formData,
      permissionList,
      select,
      submit
    }
  }
}
</script>
