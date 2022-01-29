<template>
<div class="section">
    <div class="container flex flex-col text-center justify-start">
        <h3 class="types-title neon-title text-4xl">Monstera Types</h3>
        <ul class="types">
          <li class="types-item">Monstera Deliciosa</li>
          <li class="types-item">Monstera Borsigniana</li> 
          <li class="types-item">Monstera Variegata</li>
          <li class="types-item">Monstera Adansonii</li>
          <li class="types-item">Monstera Pinnatipartita</li>
          <li class="types-item">Monstera Dubia</li>
          <li class="types-item">Monstera Siltepecana</li>
          <li class="types-item">Monstera Obliqua</li>
          <li class="types-item">Rhaphidophora Tetrasperma or “Mini Monstera”</li> 
        </ul>
    </div>
</div>
</template>
<script lang="ts">
export default {}
</script>
<style lang="scss">
@import '@/styles/mixin.scss';

    .types-title {
      font-family: 'Vibur', cursive;
    }
    .types {
      font-family: 'Vibur';
    }
    .section {
    position: absolute;
  top: 70%;
  left: 50%;
  transform: translate(-50%, -50%);
    }
    .types-item { 
      margin-top: 15px;
      font-size: 24px;
    }
</style>