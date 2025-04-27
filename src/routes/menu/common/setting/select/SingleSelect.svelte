<script lang="ts">
    import {slide} from "svelte/transition";
    import {quintOut} from "svelte/easing";
    import {createEventDispatcher} from "svelte";
    import GenericSelect from "./GenericSelect.svelte";

    export let options: string[];
    export let value: string;
    export let title: string;

    const dispatch = createEventDispatcher<{
        change: { value: string }
    }>();

    function handleOptionClick(o: string) {
        value = o;
        dispatch("change", {value: o});
    }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<GenericSelect closeOnInternalClick={true}>
    <span slot="title"><span class="title">{title}</span> {value}</span>

    <svelte:fragment slot="options">
        {#each options as o}
            <div on:click={() => handleOptionClick(o)} class="option" class:active={o === value}
                 transition:slide|global={{ duration: 200, easing: quintOut }}>{o}</div>
        {/each}
    </svelte:fragment>
</GenericSelect>

<style lang="scss">
  @use "../../../../../colors.scss" as *;

  .title {
    font-weight: 1000;
  }

  .option {
    font-weight: 1000;
    color: $menu-text-dimmed-color;
    font-size: 20px;
    border-radius: 15px;
    padding: 15px 20px;
    transition: ease-out color .2s;

    &:hover {
      color: $menu-text-color;
      text-shadow: 0 0 15px $menu-text-color;
    }

    &.active {
      color: $accent-color-2;
      text-shadow: 0 0 15px $accent-color-2;
    }
  }
</style>
