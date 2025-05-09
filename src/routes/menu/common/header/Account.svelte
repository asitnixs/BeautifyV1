<script lang="ts">
    import ToolTip from "../ToolTip.svelte";
    import {getSession, openScreen} from "../../../../integration/rest";
    import {onMount} from "svelte";
    import {listen} from "../../../../integration/ws";
    import {location} from "svelte-spa-router";

    let username = "";
    let avatar = "";
    let premium = true;

    const inAccountManager = $location === "/altmanager";
    
    async function refreshSession() {
        const session = await getSession();
        username = session.username;
        avatar = session.avatar;
        premium = session.premium;
    }

    onMount(async () => {
        await refreshSession();
    });

    listen("session", async () => {
        await refreshSession();
    });
</script>

<div class="account">
    <object data={avatar} type="image/png" class="avatar" aria-label="avatar">
        <img src="img/steve.png" alt=avatar class="avatar">
    </object>
    <div class="username">{username}</div>
    <div class="account-type">
        {#if premium}
            <span class="premium">Premium</span>
        {:else}
            <span class="offline">Offline</span>
        {/if}
    </div>
    <button class="button-change-account" disabled={inAccountManager} type="button" on:click={() => openScreen("altmanager")}>
        <ToolTip text="Change account"/>

        <img class="icon" src="img/menu/icon-pen.svg" alt="change account">
    </button>
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .account {
    background-color: rgba($hotbar-base-color, 0.25);
    width: 350px;
    padding: 15px 20px;
    border-radius: 15px;
    backdrop-filter: blur(10px);
    box-shadow: 0 0 15px black;
    align-items: center;
    display: grid;
    grid-template-areas:
        "a b c"
        "a d c";
    grid-template-columns: max-content 1fr max-content;
    column-gap: 15px;
    transition: 750ms transform ease-out;
  }

  .avatar {
    height: 68px;
    width: 68px;
    border-radius: 15px;
    grid-area: a;
    box-shadow: 0 0 15px rgba(0, 0, 0, 1);
  }

  .username {
    font-weight: 1000;
    color: $menu-text-color;
    font-size: 20px;
    grid-area: b;
    align-self: flex-end;
    text-shadow: 0 0 15px black;
  }

  .account-type {
    font-weight: 1000;
    font-size: 20px;
    grid-area: d;
    align-self: flex-start;
    text-shadow: 0 0 15px black;

    .premium {
      color: $menu-account-premium-color;
    }

    .offline {
      color: $menu-text-dimmed-color;
    }
  }

  .button-change-account {
    background-color: transparent;
    border: none;
    grid-area: c;
    position: relative;
    height: max-content;
    cursor: pointer;
    transition: 750ms transform ease-out;

    &:disabled {
      pointer-events: none;
      opacity: .5;
    }
  }
  .button-change-account:hover, .account:hover {
    transform: scale(1.1);
  }
</style>
