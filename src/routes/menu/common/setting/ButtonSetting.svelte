<script lang="ts">
    import {createEventDispatcher} from "svelte";
    import CircleLoader from "../CircleLoader.svelte";

    export let title: string;
    export let disabled = false;
    export let secondary = false;
    export let inset = false;
    export let listenForEnter = false;
    export let loading = false;

    const dispatch = createEventDispatcher();

    function handleKeyDown(e: KeyboardEvent) {
        if (!listenForEnter) {
            return;
        }
        if (e.key === "Enter") {
            dispatch("click");
        }
    }
</script>

<svelte:window on:keydown={handleKeyDown}/>
<button class="button-setting" class:inset type="button" on:click={() => dispatch("click")} {disabled} class:secondary>
    {#if loading}
        <CircleLoader/>
    {/if}
    {title}
</button>

<style lang="scss">
  @use "sass:color";
  @use "../../../../colors.scss" as *;

  .button-setting {
    position: relative;
    border: none;
    background-color: rgba($clickgui-base-color, 0.85);
    color: $menu-text-color;
    font-family: "MySFui", sans-serif;
    text-transform: uppercase;
    letter-spacing: 4px;
    text-shadow: 0 5px 5px rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 15px;
    font-size: 18px;
    transition: 750ms transform ease-out;
  }
  @property --angle{
    syntax: "<angle>";
    initial-value: 53deg;
    inherits: false;
  }
  .button-setting::before, .button-setting::after {
    content: '';
    position: absolute;
    inset: -4px;
    border-radius: 15px;
    background-image: conic-gradient(from var(--angle),
                      rgba($accent-color, 1),
                      transparent,
                      rgba($accent-color-2, 1),
                      transparent,
                      rgba($accent-color, 1));
    z-index: -1;
    animation: spin 4s linear infinite;
  }
  .button-setting::after {
    content: '';
    position: absolute;
    opacity: 1;
    inset: 0px;
    border-radius: 15px;
    background: rgba($clickgui-base-color, 0.85);
    z-index: -1;
    animation: spin 4s linear infinite;
  }
  @keyframes spin {
    from {
      --angle: 53deg;
    }
    to {
      --angle: 413deg;
    }
  }
  .button-setting:hover {
    cursor: pointer;
    transform: scale(1.1);
    box-shadow: 40px 0 75px rgba($accent-color, 0.5),
                -40px 0 75px rgba($accent-color-2, 0.5);
    transition: 750ms ease-out;
  }
  .button-setting:hover::after {
    opacity: 1;
    cursor: pointer;
  }
  @font-face {
    font-family: 'MySFui';
    src: url("/SFUI/SF-UI-Display-Regular.otf") format('truetype');
  }
</style>
