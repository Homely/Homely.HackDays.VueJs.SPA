<template lang="html">
  <div class="greeting">
    <img src="https://homely-web.appiancdn.com/assets/images/touch/icon-192x192.png" alt="Homely logo">
  </div>
</template>

<style lang="scss">
  .greeting {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    flex-direction: column;
    pointer-events: none;
    img {
      margin: 0 auto;
      position: relative;
    }
  }
</style>