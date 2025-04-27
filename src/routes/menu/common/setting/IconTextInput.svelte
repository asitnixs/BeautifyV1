<script lang="ts">
    export let icon: string;
    export let title: string;
    export let type = "text";
    export let value = "";
    export let maxLength: number | null = null;
    export let pattern: string | null = null;
</script>

<div class="icon-text-input">
    <div class="icon">
        <img src="img/menu/icon-{icon}.svg" alt={icon}>
    </div>
    {#if type === "text"}
        <input {pattern} maxlength={maxLength} class="input" spellcheck="false" type="text" placeholder={title} bind:value={value} autocomplete="off">
    {:else if type === "password"}
        <input {pattern} maxlength={maxLength} class="input" type="password" placeholder={title} bind:value={value} autocomplete="off">
    {/if}
    <div class="button-container">
        <slot />
    </div>
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .icon-text-input {
    display: grid;
    grid-template-columns: max-content 1fr max-content;
  }

  .icon {
    height: 60px;
    width: 55px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: rgba(5, 5, 5, 0.4);
    padding: 0 1px 0 20px;
    border: none;
    border-radius: 25px 0 0 25px;
  }

  .input {
    height: 60px;
    color: $menu-text-color;
    font-family: "MySFui", sans-serif;
    font-size: 18px;
    background-color: rgba(5, 5, 5, 0.4);
    border-radius: 0 35px 35px 0;
    border: none;
    padding: 0 1px 0 5px;
    width: 100%;

    &:invalid {
      border: solid 2px $menu-error-color;
    }
  }

  .button-container {
    display: flex;
    align-items: center;
  }

  @font-face {
    font-family: 'MySFui';
    src: url("/SFUI/SF-UI-Display-Regular.otf") format('truetype');
  }
</style>
