<template>
<div class="section">
    <div class="container flex flex-col text-center justify-start">
        <h3 class="types-title neon-title text-4xl">Monstera Types</h3>
        <ul class="types">
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Deliciosa</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Borsigniana</li> 
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Variegata</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Adansonii</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="30px" width="40px" />Monstera Pinnatipartita</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Dubia</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Siltepecana</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="25px" width="25px" />Monstera Obliqua</li>
          <li class="types-item flex"><img class="mr-2" src="../../src/assets/Leaf.png" height="30px" width="40px" />Rhaphidophora Tetrasperma</li> 
        </ul>
    </div>
</div>
</template>
<script lang="ts">

export default {
  components: {}
}
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
      font-size: 21px;
    }
</style>