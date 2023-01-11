<template>
  <div class="col">
    <div class="card card-body shadow border-0">
      <h4>{{ title }}</h4>
      <h5 :class="{ 'text-success': amount > 0, 'text-danger': amount < 0 }">
        {{ amount }} €
      </h5>
    </div>
  </div>
</template>

<script>
export default {
  name: "Box",
  props: {
    amount: Number,
    title: String,
  },
};
</script>
