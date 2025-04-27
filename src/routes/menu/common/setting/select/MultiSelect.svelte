<script lang="ts">
    import {slide} from "svelte/transition";
    import {quintOut} from "svelte/easing";
    import {createEventDispatcher} from "svelte";
    import GenericSelect from "./GenericSelect.svelte";

    export let options: string[];
    export let values: string[];
    export let title: string;

    const dispatch = createEventDispatcher<{
        change: { values: string[] }
    }>();

    function handleOptionClick(o: string) {
        if (values.includes(o)) {
            values = values.filter(v => v !== o);
        } else {
            values = [...values, o]
        }
        dispatch("change", {values});
    }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<GenericSelect closeOnInternalClick={false}>
    <svelte:fragment slot="title">
        {title}
    </svelte:fragment>

    <svelte:fragment slot="options">
        {#each options as o}
            <div on:click={() => handleOptionClick(o)} class="option" class:active={values.includes(o)}
                 transition:slide|global={{ duration: 200, easing: quintOut }}>
                <span>{o}</span>
            </div>
        {/each}
    </svelte:fragment>
</GenericSelect>

<style lang="scss">
  @use "../../../../../colors.scss" as *;

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
