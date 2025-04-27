<script lang="ts">
    import {type ComponentType, createEventDispatcher} from "svelte";

    let availableTabsElement: HTMLElement | undefined;

    export let tabs: {
        title: string,
        icon: string,
        component: ComponentType,
    }[];
    export let activeTab = 0;

    const dispatch = createEventDispatcher<{
        changeTab: { activeTab: number }
    }>();

    function setActiveTab(i: number) {
        activeTab = i;
        dispatch("changeTab", {activeTab});
    }
</script>

<div class="tabs">
    <div class="available-tabs" bind:this={availableTabsElement}>
        {#each tabs as {title, icon}, index}
            <button class="tab-button" class:active={tabs[activeTab].title === title}
                    on:click={() => setActiveTab(index)}>
                <img class="icon" src="img/menu/altmanager/{icon}" alt={title}>
                <span>{title}</span>
            </button>
        {/each}
    </div>

    <div style="width: {availableTabsElement?.clientWidth}px">
        <svelte:component this={tabs[activeTab].component}/>
    </div>
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .available-tabs {
    display: flex;
    column-gap: 10px;
    margin-bottom: 40px;
  }

  .tab-button {
    font-family: "Inter", sans-serif;
    background-color: rgba($menu-base-color, .025);
    color: $menu-text-color;
    backdrop-filter: blur(10px);
    padding: 10px;
    border: none;
    border-radius: 15px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    row-gap: 10px;
    cursor: pointer;
    transition: 750ms transform ease-out;

    .icon {
      height: 30px;
    }

    @property --angle{
      syntax: "<angle>";
      initial-value: 53deg;
      inherits: false;
    }

    &.active {
      border: 2px solid transparent;
      transform: scale(1.1);
      background: linear-gradient(rgba($clickgui-base-color, 0.85), rgba($clickgui-base-color, 0.85)) padding-box,
      conic-gradient(from var(--angle),
                      rgba($accent-color, 1),
                      rgba($accent-color, 0.25),
                      rgba($accent-color-2, 1),
                      rgba($accent-color, 0.25),
                      rgba($accent-color, 1)) border-box;
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

  }
</style>
