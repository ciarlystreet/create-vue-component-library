<template>
  <div>
    <img :src="imageSrc" :alt="alt" />
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      default: ""
    },
    alt: {
      type: String,
      default: ""
    },
    placeholder: {
      type: String,
      default: "https://picsum.photos/600/400"
    }
  },
  computed: {
    imageSrc() {
      return this.src ? this.src : this.placeholder;
    }
  }
};
</script>
