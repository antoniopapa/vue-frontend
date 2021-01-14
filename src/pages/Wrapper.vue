<template>
  <Nav/>

  <div class="container-fluid">
    <div class="row">
      <Menu/>

      <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
        <router-view/>
      </main>
    </div>
  </div>
</template>

<script>
import Nav from '@/components/Nav';
import Menu from '@/components/Menu';
import axios from "axios";
import {onMounted} from 'vue';
import {useRouter} from "vue-router";
import {useStore} from "vuex";

export default {
  name: "Wrapper",
  components: {
    Nav,
    Menu
  },
  setup() {
    const router = useRouter();
    const store = useStore();

    onMounted(async () => {
      try {
        const {data} = await axios.get('user');

        await store.dispatch('User/setUser', data);
      } catch (e) {
        await router.push('/login');
      }
    });
  }
}
</script>
