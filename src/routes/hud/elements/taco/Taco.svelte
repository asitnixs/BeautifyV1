<div class="stats-container">
    <span class="bps-display">
        <span class="stat-value">
            {#if playerData}
                BPS: {roundToDecimal(getBPS(lastX, playerData.position.x, lastZ, playerData.position.z, 20), 2)}
            {/if}
        </span>
    </span>
    <span class="coords-display">
        <span class="stat-value">
            {#if playerData}
                XYZ: {Math.round(playerData.position.x)} {Math.round(playerData.position.y)} {Math.round(playerData.position.z)}
            {/if}
        </span>
    </span>
</div>

<script lang="ts">
    import { listen } from "../../../../integration/ws";
    import type { ClientPlayerDataEvent } from "../../../../integration/events";
    import type { PlayerData } from "../../../../integration/types";

    let lastX = 0, lastZ = 0;
    let playerData: PlayerData | null = null;

    listen("clientPlayerData", (event: ClientPlayerDataEvent) => {
        if (playerData) {
            lastX = playerData.position.x;
            lastZ = playerData.position.z;
        }
        playerData = event.playerData;
    });

    function roundToDecimal(value: number, decimal: number) {
        return Math.round(value * Math.pow(10, decimal)) / Math.pow(10, decimal);
    }

    function getBPS(lastX: number, currentX: number, lastZ: number, currentZ: number, tickrate: number): number {
        const deltaX = currentX - lastX;
        const deltaZ = currentZ - lastZ;
        return Math.sqrt(deltaX * deltaX + deltaZ * deltaZ) * tickrate;
    }
</script>

<style lang="scss">
  .stats-container {
    display: flex;
    flex-direction: column;
    font-size: 20px;
    font-weight: 1000;
    color: white;
    gap: 4px;
    text-shadow: 0 0 15px rgba(255, 255, 255, 1);
  }

  .stat-value {
    flex: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
</style>
