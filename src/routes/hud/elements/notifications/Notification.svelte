<script lang="ts">
    export let title: string;
    export let message: string;
    export let severity: string;
</script>

<div class="notification">
    <div class="icon {severity.toString().toLowerCase()}"></div>
    <div class="title">{title}</div>
    <div class="message">{message}</div>
</div>

<style lang="scss">
  .notification {
    display: grid;
    grid-template-areas:
            "a b"
            "a c";
    grid-template-columns: max-content 1fr;
    column-gap: 10px;
    background: rgba(0, 15, 25, 0.5);
    border-radius: 5px;
    width: 300px;
    overflow: hidden;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 15, 25, 1);
  }

  .icon {
    height: 40px;
    width: 40px;
    background-position: center;
    background-repeat: no-repeat;
    border-radius: 4px;
    grid-area: a;
    transition: background-color 0.2s;
    border-radius: 15px;
    position: relative;
    background-image: url("/img/hud/notification/icon-toggle.svg");

    &.success {
      background-color: #4dac68;
      box-shadow: 0 0 15px #4dac68;
      background-image: url("/img/hud/notification/icon-success.svg");
    }

    &.error {
      background-color: #fc4130;
      box-shadow: 0 0 15px #fc4130;
      background-image: url("/img/hud/notification/icon-error.svg");
    }

    &.info {
      background-color: #4677ff;
      box-shadow: 0 0 15px #4677ff;
      background-image: url("/img/hud/notification/icon-info.svg");
    }

    &.disabled,
    &.enabled {
      &::after {
        content: "";
        position: absolute;
        height: 10px;
        width: 10px;
        border-radius: 5px;
        top: 50%;
        transform: translate(-50%, -50%);
        background: white;
        transition: all 0.2s ease-out;
      }
    }

    &.enabled {
      background-color: #4dac68;
      box-shadow: 0 0 15px #4dac68;

      &::after {
        left: 62%;
      }
    }

    &.disabled {
      background-color: #fc4130;
      box-shadow: 0 0 15px #fc4130;

      &::after {
        left: 38%;
      }
    }
  }

  .title {
    grid-area: b;
    font-size: 14px;
    color: white;
    text-shadow: 0 0 10px white;
    font-weight: 1000;
  }

  .message {
    grid-area: c;
    font-size: 12px;
    color: #cbd1e3;
    text-shadow: 0 0 10px #cbd1e3;
  }
</style>
