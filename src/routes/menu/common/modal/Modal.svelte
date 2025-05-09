<script lang="ts">
    import {fade, fly} from "svelte/transition";
    import {createEventDispatcher} from "svelte";

    export let title: string;
    export let visible: boolean;

    const dispatch = createEventDispatcher();

    function handleClick() {
        dispatch("close");
        visible = false;
    }
</script>

{#if visible}
    <div class="modal-wrapper" transition:fade|global={{duration: 200}}>
        <div class="modal" in:fly|global={{duration: 300, y: -100}} out:fly|global={{duration: 300, y: -100}}>
            <button class="button-modal-close" on:click={handleClick}>
                <img src="img/menu/icon-close.svg" alt="close">
            </button>

            <div class="title">{title}</div>

            <div class="content">
                <slot />
            </div>
        </div>
    </div>
{/if}

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .modal-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba($menu-base-color, 0.2);
    z-index: 99999;
    backdrop-filter: blur(15px);
  }

  .modal {
    background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));;
    min-width: 500px;
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.18);
    padding: 40px;
    display: flex;
    flex-direction: column;
    border-radius: 15px;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
  }

  .title {
    color: $menu-text-color;
    font-size: 30px;
    position: relative;
    width: max-content;
    align-self: center;
    margin-bottom: 80px;

    &::after {
      content: "";
      position: absolute;
      display: block;
      height: 8px;
      width: calc(90%);
      background: linear-gradient(to right, $accent-color, $accent-color-2, $accent-color, $accent-color-2, $accent-color);
      background-size: 200% auto;
      bottom: -25px;
      left: 50%;
      transform: translateX(-50%);
      border-radius: 15px;
      animation: gradientBorder 4s linear infinite;
    }
  }

  @keyframes gradientBorder {
    0% {
      background-position-x: 200%;
    }
    100% {
      background-position-x: 0%;
    }
  }

  .content {
    display: flex;
    flex-direction: column;
    row-gap: 40px;
  }

  .button-modal-close {
    height: 30px;
    width: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: transparent;
    border: solid 2px $menu-text-color;
    border-radius: 50%;
    cursor: pointer;
    top: 20px;
    right: 20px;
    position: fixed;
    transition: transform 750ms ease-out;

    &:hover {
      background-color: rgba(red, 0.5);
      transform: scale(1.1);
    }
  }

  @media screen and (max-width: 1366px) {
    .modal {
      zoom: 0.8;
    }
  }

  @media screen and (max-width: 1200px) {
    .modal {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 1100px) {
    .modal {
      zoom: 0.8;
    }
  }

  @media screen and (max-height: 700px) {
    .modal {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 540px) {
    .modal {
      zoom: 0.4;
    }
  }
</style>
