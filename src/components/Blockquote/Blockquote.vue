<template>
  <blockquote class="blockquote">
    <p class="blockquote__text">
      {{text}}
    </p>
  </blockquote>
</template>

<script>
import './Blockquote.scss';

export default {
  name: 'Blockquote',
  props: {
    text: String,
  },
};
</script>
