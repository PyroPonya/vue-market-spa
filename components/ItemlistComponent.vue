<template>
  <div class="container">
    <div @click="dropdownShow = !dropdownShow" class="btn_dropdown">
      <div class="dropdown_selected" :dropdownValue="dropdownValue">
        {{ dropdownValue }}
      </div>
      <div v-if="dropdownShow" class="dropdown_options">
        <div
          @click="
            sortPriceMin();
            dropdownValue = 'min';
          "
          class="dropdown_element"
        >
          min
        </div>
        <div
          @click="
            sortPriceMax();
            dropdownValue = 'max';
          "
          class="dropdown_element"
        >
          max
        </div>
        <div
          @click="
            sortName();
            dropdownValue = 'имя';
          "
          class="dropdown_element"
        >
          имя
        </div>
      </div>
    </div>
    <div class="itemContainer">
      <div
        @mouseenter="showRemoveBtn(el)"
        @mouseleave="hideRemoveBtn()"
        v-for="(el, id) in itemList"
        :key="id"
        class="item"
      >
        <div class="item_image" :style="{ 'background-image': `url(${el.image})` }">
          <div
            v-show="RemoveBtnId === el.id"
            @click="removeItem(el)"
            class="item_remove"
          ></div>
        </div>
        <div class="item_footer">
          <div class="item_name">{{ el.name }}</div>
          <div class="item_description">
            {{ el.description }}
          </div>
          <div class="item_price">{{ price(el) }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  setup() {
    const itemList = useItemlist();
    const dropdownValue = ref('По умолчанию');
    const RemoveBtnId = ref('');
    const dropdownShow = ref(false);
    const showRemoveBtn = (el) => {
      RemoveBtnId.value = el.id;
    };
    const hideRemoveBtn = () => {
      RemoveBtnId.value = '';
    };
    const removeItem = (el) => {
      const indexInStore = itemList.value.indexOf(el);
      itemList.value.splice(indexInStore, 1);
    };
    const price = (el) => {
      return parseInt(el.price).toLocaleString('ru-RU') + ' руб.';
    };
    // filters
    const sortPriceMin = () => {
      itemList.value = itemList.value.sort((a, b) =>
        a.price > b.price ? 1 : b.price > a.price ? -1 : 0
      );
    };
    const sortPriceMax = () => {
      itemList.value = itemList.value.sort((a, b) =>
        a.price > b.price ? -1 : b.price > a.price ? 1 : 0
      );
    };
    const sortName = () => {
      function compare(a, b) {
        if (a.name < b.name) {
          return -1;
        }
        if (a.name > b.name) {
          return 1;
        }
        return 0;
      }
      itemList.value = itemList.value.sort(compare);
    };
    return {
      itemList,
      RemoveBtnId,
      showRemoveBtn,
      hideRemoveBtn,
      removeItem,
      price,
      dropdownValue,
      dropdownShow,
      sortPriceMin,
      sortPriceMax,
      sortName,
    };
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  width: 100%;
  min-width: 1028px;
}
.btn_dropdown {
  position: relative;
  margin-bottom: 16px;
  width: 122px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  color: #b4b4b4;
}
.dropdown_options {
  position: absolute;
  width: 122px;
  display: flex;
  flex-direction: column;
  z-index: 9;
  transform: translateY(70%);
}
.dropdown_element:hover {
  cursor: pointer;
  background: #7bae73;
  color: #ffffff;
}
.dropdown_element {
  width: 122px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  color: #b4b4b4;
}
.dropdown_element:first-child {
  border-radius: 4px 4px 0 0;
}
.dropdown_element:last-child {
  border-radius: 0 0 4px 4px;
}
.btn_dropdown:hover {
  cursor: pointer;
  background: #7bae73;
  color: #ffffff;
}
.btn_dropdown .dropdown_selected {
  position: relative;
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  /* identical to box height */
  /* Greys / 500 */
  color: inherit;
}
.btn_dropdown .dropdown_selected::after {
  position: absolute;
  content: '';
  margin-left: 5px;
  height: 5px;
  width: 5px;
  border-right: 1px solid #b4b4b4;
  border-bottom: 1px solid #b4b4b4;
  transform: translateY(50%) rotate(45deg);
  border-color: inherit;
}
.itemContainer {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 16px;
}
.item {
  position: relative;
  display: flex;
  flex-direction: column;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
}
.item:hover {
  cursor: pointer;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.08), 0px 6px 10px rgba(0, 0, 0, 0.04);
}
.item_remove {
  /* display: none; */
  position: absolute;
  content: '';
  width: 32px;
  height: 32px;
  right: 0;
  top: 0;
  background-color: #ff8484;
  background-image: url(../assets/trashcan.svg);
  background-position: center;
  background-repeat: no-repeat;
  background-size: 16px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  transform: translateX(25%) translateY(-25%);
}
.item_remove:hover {
  background-color: #df442d;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
}
.item_image {
  width: 332px;
  height: 200px;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  border-radius: 4px 4px 0 0;
}
.item_footer {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 16px 16px 24px 16px;
}
.item_name {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 600;
  font-size: 20px;
  line-height: 25px;
  color: #3f3f3f;
}
.item_description {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 20px;
  color: #3f3f3f;
  height: 80px;
  width: 300px;
  overflow: hidden;
}
.item_price {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 600;
  font-size: 24px;
  line-height: 30px;
  color: #3f3f3f;
  margin: 16px 0 8px 0;
}
</style>
