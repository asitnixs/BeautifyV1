<script lang="ts">
    import {createEventDispatcher} from "svelte";

    export let title: string;
    export let icon: string;
    export let disabled = false;

    const dispatch = createEventDispatcher();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<button class="icon-text-button" on:click={() => dispatch("click")} {disabled}>
    <div class="icon">
        <img src="img/menu/{icon}" alt={title}>
    </div>
    <div class="title">{title}</div>
</button>

<style lang="scss">
    @use "../../../../colors.scss" as *;

    .icon-text-button {
      display: flex;
      border: none;

      border-radius: 15px;
      align-items: center;
      overflow: hidden;
      backdrop-filter: blur(10px);
      background: rgba(255, 255, 255, 0.014);
      transition: 750ms transform ease-out;

      &:not([disabled]):hover {
        &:hover {
          transform: scale(1.05);
          cursor: pointer;
        }
      }

      &[disabled] {
        opacity: .6;
      }
    }

    .icon {
      height: 58px;
      width: 29px;
      display: flex;
      padding: 0 0 0 35px;
      align-items: center;
      justify-content: center;
    }

    .title {
      font-size: 20px;
      font-weight: 500;
      color: $menu-text-color;
      padding: 0 30px;
    }
</style>
