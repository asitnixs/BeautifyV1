<script lang="ts">
    import { getClientInfo, getSession, getServers, getModules } from "../../../integration/rest";
    import type { ClientInfo, Session, Server } from "../../../integration/types";
    import { onMount } from "svelte";
    import { listen } from "../../../integration/ws";

    async function canShowUsername() {
        const modules = await getModules();
        return modules.some(module => module.name === "NameProtect" && !module.enabled);
    }

    let clientInfo: ClientInfo | null = null;
    let session: Session | null = null;
    let servers: Server[] | null = null;
    let showUsername = false;

    async function updateClientInfo() {
        clientInfo = await getClientInfo();
    }

    async function updateSession() {
        session = await getSession();
    }

    async function updateServers() {
        servers = await getServers();
    }

    onMount(async () => {
        await updateClientInfo();
        await updateSession();
        await updateServers();
        showUsername = await canShowUsername();
        setInterval(async () => {
            await updateClientInfo();
            await updateSession();
            await updateServers();
            showUsername = await canShowUsername();
        }, 1000);
    });

    listen("session", async () => {
        await updateSession();
    });
</script>

<div class="watermark">
    <div class="watermark-content">
        <div class="logo">
            <img src="img/clickgui/icon-client.svg" alt="icon"/>
        </div>
        {#if session && showUsername}
            <div class="information">
                <svg xmlns="http://www.w3.org/2000/svg" height="12" width="10.5" viewBox="0 0 448 512"><!--!Font Awesome Free 6.7.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2025 Fonticons, Inc.--><path fill="#ffffff" d="M224 256A128 128 0 1 0 224 0a128 128 0 1 0 0 256zm-45.7 48C79.8 304 0 383.8 0 482.3C0 498.7 13.3 512 29.7 512l388.6 0c16.4 0 29.7-13.3 29.7-29.7C448 383.8 368.2 304 269.7 304l-91.4 0z"/></svg>
                {session.username}
            </div>
        {/if}
        {#if clientInfo}
            <div class="information">
                <svg xmlns="http://www.w3.org/2000/svg" height="12" width="12" viewBox="0 0 512 512"><!--!Font Awesome Free 6.7.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2025 Fonticons, Inc.--><path fill="#ffffff" d="M64 64c0-17.7-14.3-32-32-32S0 46.3 0 64L0 400c0 44.2 35.8 80 80 80l400 0c17.7 0 32-14.3 32-32s-14.3-32-32-32L80 416c-8.8 0-16-7.2-16-16L64 64zm406.6 86.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L320 210.7l-57.4-57.4c-12.5-12.5-32.8-12.5-45.3 0l-112 112c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L240 221.3l57.4 57.4c12.5 12.5 32.8 12.5 45.3 0l128-128z"/></svg>
                {clientInfo.fps}
            </div>
        {/if}
        {#if servers && servers.length > 0}
            <div class="information">
                <svg xmlns="http://www.w3.org/2000/svg" height="12" width="10.5" viewBox="0 0 448 512"><!--!Font Awesome Free 6.7.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2025 Fonticons, Inc.--><path fill="#ffffff" d="M160 80c0-26.5 21.5-48 48-48l32 0c26.5 0 48 21.5 48 48l0 352c0 26.5-21.5 48-48 48l-32 0c-26.5 0-48-21.5-48-48l0-352zM0 272c0-26.5 21.5-48 48-48l32 0c26.5 0 48 21.5 48 48l0 160c0 26.5-21.5 48-48 48l-32 0c-26.5 0-48-21.5-48-48L0 272zM368 96l32 0c26.5 0 48 21.5 48 48l0 288c0 26.5-21.5 48-48 48l-32 0c-26.5 0-48-21.5-48-48l0-288c0-26.5 21.5-48 48-48z"/></svg>
                {servers[0].ping}
            </div>
        {/if}
    </div>
</div>

<style lang="scss">
  @import "../../../colors.scss";

  .watermark {
    padding: 5px;
    background: rgba(0, 15, 25, 0.5);
    box-shadow: 0 0 20px rgba($scoreboard-base-color, 1);
    border-radius: 10px;
    color: white;
    font-size: 14px;
    display: flex;
    align-items: center;
    min-width: 150px;
  }

  .watermark-content {
    display: flex;
    align-items: center;
  }

  .logo {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 28px;
    height: 28px;
    padding: 4px;
    border-radius: 50%;
  }

  .information {
    margin: 0 8px;
  }

  img {
    width: 20px;
    height: 20px;
  }
</style>
