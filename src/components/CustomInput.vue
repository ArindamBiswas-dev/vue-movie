<template>
  <form class="space-x-4" @submit.prevent="searchMovie">
    <input
      type="text"
      class="bg-gray-300 text-2xl px-4 py-3 rounded-lg outline-none"
      :value="modelValue"
      @input="updateValue"
    />
    <button
      class="
        px-5
        py-3
        bg-green-500
        hover:bg-green-600
        rounded-lg
        text-white text-xl
        font-semibold
      "
    >
      Search
    </button>
  </form>
</template>

<script>
export default {
  name: 'CustomInput',
  props: {
    modelValue: String,
  },
  setup(props, context) {
    function updateValue(event) {
      context.emit('update:modelValue', event.target.value);
    }

    function searchMovie() {
      // emmiting the search event at parent comonent
      context.emit('search');
    }

    return {
      updateValue,
      searchMovie,
    };
  },
};
</script>

<style scoped></style>
