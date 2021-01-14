<template>
  <nav>
    <ul class="pagination">
      <li class="page-item">
        <a class="page-link" href="javascript:void(0)" @click="prev">Previous</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="javascript:void(0)" @click="next">Next</a>
      </li>
    </ul>
  </nav>
</template>

<script lang="ts">
import {ref, watch, SetupContext} from "vue";

export default {
  name: "Paginator",
  emits: ['page-changed'],
  props: {
    lastPage: Number
  },
  setup(props: { lastPage: number }, context: SetupContext) {
    const page = ref(1);

    watch(page, () => {
      context.emit('page-changed', page.value);
    });

    const next = () => {
      if (page.value < props.lastPage) {
        page.value++;
      }
    }

    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    }
    return {
      next,
      prev
    }
  }
}
</script>
