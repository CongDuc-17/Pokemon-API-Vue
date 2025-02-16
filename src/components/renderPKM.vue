<template>
  <div class="items">
    <div v-for="item in props.pokemons" class="item" :key="item">
      <div class="item__id">#{{ getIDPokemon(item.url) }}</div>
      <div
        class="item__image"
        :style="{
          backgroundImage: `url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getIDPokemon(
            item.url
          )}.png')`,
        }"
      ></div>

      <div class="item__name">
        {{ item.name }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  pokemons: Array, // Nhận danh sách Pokémon từ cha
});

function getIDPokemon(url) {
  const parts = url.split("/");
  return parseInt(parts[parts.length - 2], 10);
} //Hàm getIDPokemon trích xuất ID của Pokémon từ URL.
</script>

<style scoped>
.items {
  display: flex;
  justify-content: center;
  gap: 5px;
  flex-wrap: wrap;
  .item {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 5px 5px;
    cursor: pointer;
    border-radius: 10px;
    box-shadow: #f4f4f4 0 7px 29px;
    text-transform: capitalize;
    transition: 0.3s;
  }

  .item:hover {
    box-shadow: #39393f33 0 7px 29px;
    transition: 0.3s;
  }

  .item__image {
    width: 100px;
    height: 100px;
    background-size: cover;
    background-position: center;
  }
}
</style>
