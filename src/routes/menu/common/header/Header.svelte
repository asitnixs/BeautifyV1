<script lang="ts">
    import Account from "./Account.svelte";
    import Notifications from "./Notifications.svelte";
</script>

<div class="header">

    <Notifications />

    <Account/>
</div>

<style lang="scss">
  .header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
    align-items: center;
  }
</style>
