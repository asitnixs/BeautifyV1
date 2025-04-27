<script lang="ts">
    export let text: string;
</script>

<div class="tag">
    {text}
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .tag {
    background-color: rgba($menu-base-color, 0.25);
    color: $menu-text-color;
    font-size: 12px;
    padding: 3px 7px;
    border-radius: 15px;
    margin-left: 10px;
    transition: ease-out color .2s, ease background-color .2s;
    transform: translateY(-3px);
  }
</style>
