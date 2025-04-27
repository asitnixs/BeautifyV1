<div class="button-container">
    <slot />
</div>

<style lang="scss">
    @use "../../../../colors.scss" as *;

    .button-container {
      background-color: rgba($menu-base-color, 0.1);
      padding: 10px 10px;
      width: max-content;
      border-radius: 15px;
      display: flex;
      backdrop-filter: blur(10px);
      box-shadow: 0 0 15px rgba($menu-base-color, 0.9);
      column-gap: 10px;
      transition: 750ms transform ease-out;
    }
    .button-container:hover {
      transform: scale(1.1);
      backdrop-filter: blur(10px);
      box-shadow: 0 0 15px rgba($menu-base-color, 0.9);
    }
</style>
