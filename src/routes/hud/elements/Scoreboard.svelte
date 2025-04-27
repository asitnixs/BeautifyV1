<script lang="ts">
    import {listen} from "../../../integration/ws";
    import type {PlayerData, Scoreboard} from "../../../integration/types";
    import TextComponent from "../../menu/common/TextComponent.svelte";
    import type {ClientPlayerDataEvent} from "../../../integration/events";

    let scoreboard: Scoreboard | null = null;

    listen("clientPlayerData", (e: ClientPlayerDataEvent) => {
        const playerData: PlayerData = e.playerData;
        scoreboard = playerData.scoreboard;
    });
</script>

{#if scoreboard}
    <div class="scoreboard">
        {#if scoreboard.header}
            <div class="header">
                <TextComponent fontSize={14} allowPreformatting={true} textComponent={scoreboard.header}/>
            </div>
        {/if}
        <div class="entries">
            {#each scoreboard.entries as {name}}
                <div class="row">
                    <TextComponent fontSize={14} allowPreformatting={true} textComponent={name}/>
                </div>
            {/each}
        </div>
    </div>
{/if}

<style lang="scss">
  @use "../../../colors.scss" as *;

  .scoreboard {
    width: max-content;
    border-radius: 10px;
    overflow: hidden;
    font-size: 14px;
    box-shadow: 0 0 20px rgba($scoreboard-base-color, 1);
  }

  .entries {
    background-color: rgba($scoreboard-base-color, 0.5);
    padding: 10px;
  }

  .row {
    display: flex;
    column-gap: 15px;
    justify-content: space-between;
  }

  .header {
    text-align: center;
    background-color: rgba($scoreboard-base-color, 0.65);
    padding: 7px 10px;
  }
</style>
