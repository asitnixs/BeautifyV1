<script lang="ts">
    import ToolTip from "../ToolTip.svelte";
    import {createEventDispatcher} from "svelte";

    export let title: string;
    export let icon: string;

    const dispatch = createEventDispatcher();
</script>

<button class="button" type="button" on:click={() => dispatch("click")}>
    <ToolTip text={title} color="black"/>
    <img class="icon" src="img/menu/icon-{icon}.svg" alt={icon}>
</button>

<style lang="scss">
  .button {
    background-color: transparent;
    border-radius: 15px;
    border: none;
    cursor: pointer;
    position: relative;
    margin-left: 15px;
    transition: 750ms transform ease-out;
  }
  .button:hover {
    transform: scale(1.1);
  }

  .icon {
    height: 27px;
    width: 27px;
  }
</style>
