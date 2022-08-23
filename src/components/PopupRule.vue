<template>
  <div class="wrapper-popup" v-bind:class="getClassPopup">
    <div class="rule">
      <h1>Game Rule</h1>
      <p>
        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Officia facere
        labore in eaque repellendus autem aliquid, velit, voluptatibus tempore
        officiis itaque est? Modi soluta quae tempora dolor eius quia nostrum
        nobis illo voluptatibus fuga ullam itaque, voluptatem repudiandae
        ratione. Deserunt ea iure deleniti excepturi tempore modi quod
        laboriosam, facilis aperiam illum facere
      </p>
      <button v-on:click="confirm">Get it</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup",
  props: {
    isOpenPopup: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {};
  },
  methods: {
    confirm() {
      this.$emit("handleConfirm");
    }
  },
  computed: {
    getClassPopup: function() {
      return {
        "open-popup": this.isOpenPopup
      };
    }
  }
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(84 70 70 / 32%);
  visibility: hidden;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}
.rule {
  position: absolute;
  background-color: white;
  width: 300px;
  padding: 20px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
