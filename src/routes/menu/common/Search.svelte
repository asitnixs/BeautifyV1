<script lang="ts">
    import {createEventDispatcher} from "svelte";

    let value = "";

    const dispatch = createEventDispatcher<{
        search: { query: string }
    }>();
</script>

<input class="search" type="text" placeholder="Search..." bind:value={value}
       on:input={() => dispatch("search", {query: value})} spellcheck="false">

<style lang="scss">
  @use "../../../colors.scss" as *;

  .search {
    background-color: rgba($menu-base-color, .025);
    border: none;
    color: $menu-text-color;
    text-shadow: 0 0 5px white;
    font-family: "Inter", sans-serif;
    font-size: 20px;
    border-radius: 15px;
    backdrop-filter: blur(10px);
    box-shadow: 0 0 15px black;
    background-image: url("/img/menu/icon-search.svg");
    background-repeat: no-repeat;
    background-position: 20px center;
    padding: 20px 20px 20px 60px;
    flex: 1;
  }
</style>
