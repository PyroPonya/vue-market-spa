<template>
  <div class="container">
    <div class="title">Добавление товара</div>
    <form action="#" @submit.prevent="true" class="itemForm">
      <div
        class="invalidMessage"
        :style="
          validFields.name ? { '--errorDisplay': 'none' } : { '--errorDisplay': 'block' }
        "
      >
        <label for="itemName">Наименование товара</label>
        <input
          v-model="formData.name"
          :class="validFields.name ? '' : 'invalid'"
          type="text"
          id="itemName"
          placeholder="Введите наименование товара"
        />
      </div>
      <label for="itemDescription">Описание товара</label>
      <textarea
        v-model="formData.description"
        id="itemDescription"
        placeholder="Введите описание товара"
      />
      <label for="itemImage">Ссылка на изображение товара</label>
      <div
        class="invalidMessage"
        :style="
          validFields.image ? { '--errorDisplay': 'none' } : { '--errorDisplay': 'block' }
        "
      >
        <input
          v-model="formData.image"
          :class="validFields.image ? '' : 'invalid'"
          type="text"
          id="itemImage"
          placeholder="Введите ссылку"
        />
      </div>
      <div
        class="invalidMessage"
        :style="
          validFields.price ? { '--errorDisplay': 'none' } : { '--errorDisplay': 'block' }
        "
      >
        <label for="itemPrice">Цена товара</label>
        <input
          v-model="formData.price"
          :class="validFields.price ? '' : 'invalid'"
          type="number"
          min="0"
          id="itemPrice"
          placeholder="Введите цену"
        />
      </div>
      <button v-if="enabled" @click="addItem()" type="submit" class="btnEnabled">
        Добавить товар
      </button>
      <button v-else type="submit" class="btnDisabled">Добавить товар</button>
    </form>
  </div>
</template>

<script>
export default {
  setup() {
    const itemList = useItemlist();
    /* form data */
    const enabled = ref(false);
    const formData = ref({
      name: '',
      description: '',
      image: '',
      price: '',
    });
    const validFields = ref({
      name: true,
      image: true,
      price: true,
    });
    const errorDisplay = ref('none');
    // adding items
    const getNewId = () => {
      return itemList.value.at(-1) ? parseInt(itemList.value.at(-1).id) : 0;
    };
    const addItem = () => {
      const createdItem = {
        id: getNewId() + 1,
        name: formData.value.name,
        description: formData.value.description,
        image: formData.value.image,
        price: formData.value.price,
      };
      itemList.value.push(createdItem);
    };
    // validation form watcher
    watch(
      () => formData,
      () => {
        // name
        if (formData.value.name !== '' && formData.value.name) {
          validFields.value.name = true;
        } else {
          validFields.value.name = false;
        }
        // image
        if (formData.value.image !== '' && formData.value.image) {
          validFields.value.image = true;
        } else {
          validFields.value.image = false;
        }
        // price
        if (formData.value.price !== '' && formData.value.price) {
          validFields.value.price = true;
        } else {
          validFields.value.price = false;
        }
        // total
        if (
          validFields.value.name === true &&
          validFields.value.image === true &&
          validFields.value.price === true
        ) {
          enabled.value = true;
        } else {
          enabled.value = false;
        }
      },
      { deep: true }
    );
    return {
      enabled,
      formData,
      validFields,
      errorDisplay,
      addItem,
    };
  },
};
</script>

<style scoped>
* {
  --errorDisplay: 'none';
}
.container {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.title {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
  color: #3f3f3f;
  margin-bottom: 16px;
}
.itemForm {
  position: sticky;
  top: 24px;
  display: flex;
  flex-direction: column;
  width: 332px;
  min-height: 440px;
  padding: 24px;
  align-items: flex-start;
  justify-content: center;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  border: none;
}
.itemForm label {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 10px;
  line-height: 13px;
  /* identical to box height */
  letter-spacing: -0.02em;
  /* Temp / Darks / Lesser */
  color: #49485e;
  margin-bottom: 4px;
}
.itemForm label[for='itemName'],
.itemForm label[for='itemImage'],
.itemForm label[for='itemPrice'] {
  display: flex;
  flex-direction: row;
}
.itemForm label[for='itemName']::after,
.itemForm label[for='itemImage']::after,
.itemForm label[for='itemPrice']::after {
  content: '';
  display: block;
  height: 4px;
  width: 4px;
  background-color: #ff8484;
  border-radius: 4px;
  top: 0;
  transform: translateY(-50%);
}
.itemForm input,
.itemForm textarea {
  margin-bottom: 16px;
  padding: 10px 16px 11px 16px;
  width: 284px;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: none;
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  color: #3f3f3f;
}
.itemForm input:focus,
.itemForm textarea:focus {
  outline: none;
}
.itemForm input::placeholder,
.itemForm textarea::placeholder {
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  /* identical to box height */
  /* Greys / 500 */
  color: #b4b4b4;
}
.itemForm textarea {
  resize: none;
  height: 108px;
}
.itemForm input {
  height: 36px;
}
.itemForm input.invalid {
  outline: 1px solid #ff8484;
}
/* hide spinners on num input */
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
/* Firefox */
input[type='number'] {
  -moz-appearance: textfield;
}
/*******************************/
.invalidMessage::after {
  display: var(--errorDisplay);
  position: absolute;
  transform: translateY(-100%);
  content: 'Поле является обязательным';
  font-family: 'Source Sans Pro', sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 8px;
  line-height: 10px;
  /* identical to box height */
  letter-spacing: -0.02em;
  color: #ff8484;
}
.itemForm > button {
  width: 284px;
  height: 36px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  font-family: 'Inter', sans-serif;
  font-style: normal;
  font-weight: 600;
  font-size: 12px;
  line-height: 15px;
  /* identical to box height */
  border: none;
}
.itemForm > button:hover {
  cursor: pointer;
}
.itemForm > button.btnEnabled {
  background: #7bae73;
  color: #ffffff;
}
.itemForm > button.btnEnabled:active {
  background: #6ead64;
  color: #ffffff;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
}
.itemForm > button.btnDisabled {
  background: #eeeeee;
  color: #b4b4b4;
  cursor: not-allowed;
}
</style>
