<script lang="ts">
    import {createEventDispatcher} from "svelte";

    export let value: boolean;
    export let name: string;

    const dispatch = createEventDispatcher();
</script>

<label class="switch-container">
    <div class="switch">
        <input type="checkbox" bind:checked={value} on:change={() => dispatch("change")}/>
        <span class="slider"></span>
    </div>

    <div class="name">{name}</div>
</label>

<style lang="scss">
  @use "sass:color";
  @use "../../../../colors.scss" as *;

  .switch-container {
    display: flex;
    align-items: center;
    cursor: pointer;
  }

  .name {
    font-weight: 1000;
    color: $clickgui-text-color;
    font-size: 12px;
    margin-left: 7px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .slider {
    position: absolute;
    top: 2px;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: color.adjust($clickgui-text-color, $lightness: -55%);
    transition: ease 0.4s;
    height: 8px;
    border-radius: 4px;
    box-shadow: 0 0 15px rgba(255, 255, 255, 0.75);

    &::before {
      position: absolute;
      content: "";
      height: 12px;
      width: 12px;
      top: -2px;
      left: 0;
      background-color: $clickgui-text-color;
      transition: ease 0.4s;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(255, 255, 255, 0.75);
    }
  }

  .switch {
    position: relative;
    width: 22px;
    height: 12px;

    input {
      display: none;
    }

    input:checked + .slider {
      background: color.adjust($clickgui-main-color-1, $saturation: -60%, $lightness: -15%);
      box-shadow: 0 0 15px rgba($accent-color-2, 0.75);
    }

    input:checked + .slider:before {
      transform: translateX(10px);
      background: linear-gradient(90deg, $accent-color, $accent-color-2);
      box-shadow: 0 0 15px rgba($accent-color-2, 0.75);
    }
  }
</style>
