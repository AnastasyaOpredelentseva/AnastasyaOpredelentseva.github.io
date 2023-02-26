<template>
  <div class="popup">
    <div class="popup-inner">
      <slot />
      <button class="button" @click="TogglePopup()">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["TogglePopup"],
};
</script>
