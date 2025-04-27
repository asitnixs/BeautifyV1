<script lang="ts">
    import {fade, fly, scale} from "svelte/transition";
    import {createEventDispatcher} from "svelte";
    import {backIn, backOut} from "svelte/easing";

    export let title: string;
    export let icon: string;
    let isMenuActive = false;
    export let index: number;

    let hovered = false;

    const dispatch = createEventDispatcher();
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="main-button" on:mouseenter={() => hovered = true} on:mouseleave={() => hovered = false} on:click={() => hovered = false}
    on:click={() => dispatch("click")} out:fly|global={{duration: 500, y: -500, delay: index * 100, easing: backIn}}
    in:fly|global={{duration: 500, y: -500, delay: index * 100, easing: backOut}}>

    <div class="icon">
        {#if !hovered}
            <img transition:fade={{duration: 200}} src="img/menu/icon-{icon}.svg" alt={icon}>
        {:else}
            <img transition:fade={{duration: 200}} src="img/menu/icon-{icon}-hover.svg" alt={icon}>
        {/if}
    </div>

    <div class="title">{title}</div>

    <div class="wrapped-content">
        <slot parentHovered={hovered}/>
    </div>
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .main-button {
    background-color: rgba(255, 255, 255, 0.014);
    width: 275px;
    height: 75px;
    padding: 5px 5px;
    display: grid;
    grid-template-columns: max-content 1fr max-content;
    align-items: center;
    cursor: pointer;
    border-radius: 15px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
    column-gap: 15px;
    transition: 750ms transform ease-out;

    &:hover {
      transform: scale(1.1);

      .icon {
        background-color: $menu-text-color;
        box-shadow: 0 0 15px rgba(0, 0, 0, 0.25);
      }
    }
  }

  .icon {
    background: linear-gradient(135deg,
            $clickgui-main-color-1,
            $clickgui-main-color-2);
    width: 64px;
    height: 64px;
    border-radius: 50%;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.25);
    transition: ease-out background-color 0.2s;
    position: relative;

    img {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  }

  .title {
    font-size: 27px;
    color: $menu-text-color;
    font-weight: bolder;
    text-shadow: 2.5px 2.5px 7.5px black;
  }
</style>
