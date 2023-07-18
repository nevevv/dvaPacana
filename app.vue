<template>
  <div class="container">
    <div class="left-block">
      <h2>Вещи у пользователя</h2>
      <div class="selected-items">
        <div
          v-for="item in selectedItems"
          :key="item.id"
          class="selected-item"
        >
          {{ item.name }}
          <span class="delete-icon" @click="removeItem(item)"></span>
        </div>
      </div>
      <p>Выбрано: {{ selectedItemsCount }} / 6</p>
      <p class="mt-1" v-if="selectedItemsCount === 6">Максимальное количество выбранных вещей достигнуто!</p>
    </div>

    <div class="right-block">
      <h2>Вещи на выбор</h2>
      <div class="selectable-items">
        <div
          v-for="item in selectableItems"
          :key="item.id"
          class="selectable-item"
          :class="{ 'active-item': isSelected(item) }"
          @click="selectItem(item)"
        >
          {{ item.name }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

interface Item {
  id: number;
  name: string;
}

const selectedItem = ref<Item | null>(null);
const selectedItems = ref<Item[]>([]);
const selectableItems = ref<Item[]>([
  {
    id: 1,
    name: "Shoes 1",
  },
  {
    id: 2,
    name: "Shoes 2",
  },
  {
    id: 3,
    name: "Shoes 3",
  },
  {
    id: 4,
    name: "Shoes 4",
  },
  {
    id: 5,
    name: "T-shirt 1",
  },
  {
    id: 6,
    name: "T-shirt 2",
  },
  {
    id: 7,
    name: "T-shirt 3",
  },
  {
    id: 8,
    name: "T-shirt 4",
  },
]);

const selectItem = (item: Item): void => {
  if (selectedItem.value === item) {
    return;
  }
  if (selectedItems.value.length < 6) {
    selectedItems.value.push(item);
    selectedItem.value = item;
  } else {
    alert("Максимальное количество выбранных вещей достигнуто!");
  }
};

const removeItem = (item: Item): void => {
  const index = selectedItems.value.findIndex((selectedItem) => selectedItem === item);
  if (index !== -1) {
    selectedItems.value.splice(index, 1);
    if (selectedItem.value === item) {
      selectedItem.value = null;
    }
  }
};

const isSelected = (item: Item): boolean => {
  return selectedItems.value.includes(item);
};

const selectedItemsCount = computed(() => {
  return selectedItems.value.length;
});
</script>

<style scoped>
.mt-1 {
  margin-top: 27px;
}
.container {
  display: flex;
  justify-content: space-between;
  margin-top: 50px;
}

.left-block,
.right-block {
  width: 200px;
  height: 300px;
  border: 1px solid black;
  padding: 10px;
}

.selected-items {
  margin-top: 20px;
}

.selected-item {
  margin-bottom: 10px;
  cursor: pointer;
  position: relative;
}

.delete-icon {
  position: absolute;
  top: 5px;
  right: 5px;
  width: 20px;
  height: 20px;
  cursor: pointer;
  background-color: transparent;
  border: none;
}

.delete-icon::before,
.delete-icon::after {
  content: '';
  position: absolute;
  width: 12px;
  height: 2px;
  background-color: black;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.delete-icon::before {
  transform: translate(-50%, -50%) rotate(45deg);
}

.delete-icon::after {
  transform: translate(-50%, -50%) rotate(-45deg);
}

.selectable-item {
  cursor: pointer;
}

.active-item {
  font-weight: bold;
  background-color: lightblue;
}
</style>
