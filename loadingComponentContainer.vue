<template>
  <div>
    <div class="general-message is-success is-small-popup">
      <h2 class="general-message-title load-points-animation">Cargando<span>.</span><span>.</span><span>.</span></h2>
    </div>
    <div class="captureClient_component-black_blanket"></div>
  </div>
</template>

<script>
    export default {
        name: "loadingComponent",
        data: () => ({
        }),
    }
</script>



<style scoped>
    .general-message.is-success.is-small-popup {
        max-width: 162px;
        z-index: 1075;
    }
    .general-message {
        background: white;
        max-width: 260px;
        width: 100%;
        border-radius: 5px;
        box-shadow: 2px 4px 13px 0px rgba(0,0,0,0.29);
        left: 50%;
        top: 50%;
        position: fixed;
        transform: translate(-50%, -50%);
        z-index: 1070;
        padding: 30px;
    }
    .is-small-popup h2.general-message-title.load-points-animation {
        color: #2A3F54;
    }
    .load-points-animation span {
        -webkit-animation-name: blink;
        animation-name: blink;
        -webkit-animation-duration: 1.4s;
        animation-duration: 1.4s;
        -webkit-animation-iteration-count: infinite;
        animation-iteration-count: infinite;
        -webkit-animation-fill-mode: both;
        animation-fill-mode: both;
    }
    .captureClient_component-black_blanket {
        z-index: 1054;
        background: #151515ba;
        left: 0;
        right: 0;
        position: fixed;
        bottom: 0;
        top: 0;
    }
</style>
