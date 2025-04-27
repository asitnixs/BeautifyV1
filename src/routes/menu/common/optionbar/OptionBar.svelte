<script lang="ts">
    import {fly} from "svelte/transition";
</script>

<div class="option-bar" transition:fly|global={{duration: 700, x: 1000}}>
    <slot />
</div>

<style lang="scss">
    @use "../../../../colors.scss" as *;

    .option-bar {
      background-color: rgba($menu-base-color, 0.4);
      padding: 15px 15px;
      display: flex;
      border-radius: 15px;
      align-items: center;
      column-gap: 30px;
      margin-bottom: 25px;
      box-shadow: 0 0 15px black;
    }
</style>
