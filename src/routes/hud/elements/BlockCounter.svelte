<script lang="ts">
    import {listen} from "../../../integration/ws";
    import {fly} from "svelte/transition";
    import type {BlockCountChangeEvent} from "../../../integration/events";
    import {mapToColor} from "../../../util/color_utils";

    let count: number | undefined;

    listen("blockCountChange", (data: BlockCountChangeEvent) => {
        count = data.count;
    });

</script>

{#if count !== undefined}
    <div class="counter" in:fly={{ y: -5, duration: 200 }} out:fly={{ y: -5, duration: 200 }}>
        Blocks: <span style="color: {mapToColor(count)}">{count}</span>
    </div>
{/if}

<style lang="scss">
  @use "../../../colors.scss" as *;

  .counter {
    background-color: rgba($blockcounter-base-color, 0.5);
    border-radius: 10px;
    white-space: nowrap;
    padding: 5px 8px;
    font-weight: 1000;
    transform: translate(-100%);
    color: white;
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.75);
    box-shadow: 0 0 20px rgba($scoreboard-base-color, 1);
  }
</style>
