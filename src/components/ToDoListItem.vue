<script setup>
defineProps(["ch","handlerD","handlerF"]);
</script>

<template>
<li>{{ ch.pourAfficher() }}
 <button @click="handlerD(ch.id)">Delete</button> //si on recup "idx" c'est la position ds le tableau nous on veut recup l'id de la chose
 <button @click="handlerF(ch.id,ch.fait,ch.texte)">faire</button>
</li>
</template>

<style>
</style>