<script lang="ts">
    import {listen} from "../../../integration/ws";
    import type {ClientPlayerDataEvent} from "../../../integration/events";
    import type {StatusEffect} from "../../../integration/types";

    let effects: StatusEffect[] = [];

    listen("clientPlayerData", (event: ClientPlayerDataEvent) => {
        effects = event.playerData.effects;
    });

    function formatTime(duration: number): string {
        return new Date(((duration / 20) | 0) * 1000).toISOString().substring(14, 19);
    }

    function convertToRoman(n: number): string {
        switch (n) {
            case 0: return "1";
            case 1: return "2";
            case 2: return "3";
            case 3: return "4";
            case 4: return "5";
            case 5: return "6";
            case 6: return "7";
            case 7: return "8";
            case 8: return "9";
            case 9: return "10";
            default: return "10+";
        }
    }
</script>

<div class="effects">
  <span class="header">Potion Status</span>
    {#each effects as e}
        <div class="effect">
            <img class="effect-icon" src={"img/hud/effects/icon-" + e.localizedName.replace(' ', '_').toLowerCase() + ".svg"}/>
            <div class="text">
                <span class="name" style="color: {'#' + e.color.toString(16)}">
                    {e.localizedName} {convertToRoman(e.amplifier)}:
                </span>
                <span class="duration">{formatTime(e.duration)}</span>
            </div>
        </div>
    {/each}
</div>

<style lang="scss">
  @import "../../../colors.scss";

  .header {
    padding: 2.5px;
    display: flex;
    justify-content: center;
    align-content: center;
    text-align: center;
    color: white;
    font-weight: 1000;
    font-size: 20px;
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
  }

  .effects {
    background-color: rgba(0, 15, 25, 0.5);
    box-shadow: 0 0 20px rgba($scoreboard-base-color, 1);
    border-radius: 10px;
  }

  .effect {
    font-weight: 1000;
    font-size: 15px;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 5px;

    .effect-icon {
      width: 20px;
      height: 20px;
      image-rendering: pixelated;
    }

    .duration {
      color: white;
      text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
    }
  }
</style>
