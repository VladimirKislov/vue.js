<template>
  <div class="root">
    <!-- up menu -->
    <div class="up__menu">
      <h2 class="up__menu-title">
        Добавление товара
      </h2>
      <select v-model="select" class="up__menu-select" @click.prevent="sortArray">
        <option class="up__menu-option">
          По умолчанию
        </option>
        <option class="up__menu-option">
          Цена по возврастанию
        </option>
        <option class="up__menu-option">
          Цена по убыванию
        </option>
      </select>
    </div>
    <!-- section main -->
    <section class="main">
      <!-- form -->
      <form class="main__add-product" @submit.prevent="onSubmit">
        <label for="product" class="main__add-product-label">
          <span class="main__add-product-label-span">
            Наименование товара
            <span class="main__add-product-star">*</span>
          </span>
          <input
            id="product"
            v-model="product"
            class="main__add-product-input"
            placeholder="Введите наименование товара"
            type="text"
            :class="{active: isValidProduct, error: isErrorProduct}"
            @blur="productBlur"
          >
          <p v-show="isVisibleProduct" class="main__add-product-error">
            Поле является обязательным
          </p>
        </label>
        <label for="" class="main__add-product-label">Описание товара
          <textarea
            v-model="description"
            class="main__add-product-textarea"
            rows="10"
            cols="37"
            name="textarea"
            placeholder="Введите описание товара"
          />
        </label>
        <label for="link" class="main__add-product-label">
          <span class="main__add-product-label-span">
            Ссылка на изображение товара
            <span class="main__add-product-star">*</span>
          </span>
          <input
            id="link"
            v-model="link"
            class="main__add-product-input"
            type="text"
            placeholder="Введите ссылку"
            :class="{active: isValidLink, error: isErrorLink}"
            @blur="linkBlur"
          >
          <p v-show="isVisibleLink" name="link" class="main__add-product-error">
            Поле является обязательным
          </p>
        </label>

        <label for="price" class="main__add-product-label">
          <span class="main__add-product-label-span">
            Цена товара
            <span class="main__add-product-star">*</span>
          </span>
          <input
            id="price"
            v-model="price"
            class="main__add-product-input"
            type="text"
            name="price"
            placeholder="Введите цену"
            :class="{active: isValidPrice, error: isErrorPrice}"
            @blur="priceBlur"
          >
          <p v-show="isVisiblePrice" name="price" class="main__add-product-error">
            Поле является обязательным
          </p>
        </label>

        <input
          class="main__add-product-btn"
          type="submit"
          value="Добавить товар"
          :disabled="(isValidProduct === true && isValidLink === true && isValidPrice === true) ? false : true"
        >
      </form>
      <!-- products card -->
      <div class="main__show-product">
        <transition-group name="list" tag="div">
          <div v-for="(prod, item) in arrayView" :key="prod.id" class="main__show-product-wrapper" @mouseover="isHover = item" @mouseleave="isHover = null">
            <img
              class="main__show-product-img"
              :src="prod.imageData.includes('http') ? `${prod.imageData}` : require(`../assets/image/31.jpg`)"
              alt="image"
            >
            <div class="main__show-product-info">
              <h3 class="main__show-product-info-title">
                {{ prod.titleData }}
              </h3>
              <p class="main__show-product-info-description">
                {{ prod.descriptionData }}
              </p>
              <p class="main__show-product-info-price">
                {{ prod.priceData }}<span> руб.</span>
              </p>
            </div>
            <div v-show="isHover === item" class="main__show-product-remove" @click="removeArray(item)">
              <img :src="require('../assets/image/16.png')" alt="removeImage">
            </div>
          </div>
        </transition-group>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'NuxtHome',
  data () {
    return {
      isVisibleProduct: false,
      isVisibleLink: false,
      isVisiblePrice: false,
      disabled: true,
      isErrorProduct: false,
      isErrorLink: false,
      isErrorPrice: false,
      isHover: null,
      product: '',
      link: '',
      price: '',
      description: '',
      isValidProduct: false,
      isValidLink: false,
      isValidPrice: false,
      select: 'По умолчанию',
      count: 0,
      arrayView: [],
      arrayProducts: [
        {
          id: 1,
          imageData: '',
          titleData: 'Наименование товара',
          descriptionData: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
          priceData: '10 000',
          realPrice: '10000'
        },
        {
          id: 2,
          imageData: '',
          titleData: 'Наименование товара',
          descriptionData: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
          priceData: '10 000',
          realPrice: '10000'
        },
        {
          id: 3,
          imageData: '',
          titleData: 'Наименование товара',
          descriptionData: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
          priceData: '10 000',
          realPrice: '10000'
        },
        {
          id: 4,
          imageData: 'https://img.championat.com/news/big/n/f/kak-pravilno-meditirovat-sovety-praktika_157684379279536979.jpg',
          titleData: 'Медитация',
          descriptionData: 'Между тем медитация — это серьезная духовная практика, которая требует специального обучения и подготовки, а желательно и опытного руководителя, под чьим присмотром ее осваивали бы неофиты.',
          priceData: '12 000',
          realPrice: '12000'
        },
        {
          id: 5,
          imageData: 'http://www.rosphoto.com/images/u/articles/1510/7_5.jpg',
          titleData: 'Среди облаков',
          descriptionData: 'Фото: Петрова Елена Валерьевна, «Среди облаков». Фото в Facebook 348 лайков ; Vkontakte 40 лайков; Instagram 466 лайков',
          priceData: '4 200',
          realPrice: '4200'
        },
        {
          id: 6,
          imageData: 'http://www.rosphoto.com/images/u/articles/1510/29_1.jpg',
          titleData: 'Закат',
          descriptionData: 'Фото: Анастасия Костакова, «Закат». Фото в Facebook 472 лайка; Vkontakte 49 лайков; Instagram 564 лайка',
          priceData: '1 700',
          realPrice: '1700'
        }
      ]
    }
  },
  mounted () {
    if (localStorage.getItem('arrayProducts')) {
      try {
        this.arrayProducts = JSON.parse(localStorage.getItem('arrayProducts'))
        this.arrayView = JSON.parse(localStorage.getItem('arrayProducts'))
      } catch (elem) {
        localStorage.removeItem('arrayProducts')
      }
    } else {
      const parsed = JSON.stringify(this.arrayProducts)
      localStorage.setItem('arrayProducts', parsed)
      this.arrayView = JSON.parse(localStorage.getItem('arrayProducts'))
    }
  },
  methods: {
    productBlur () {
      if (this.product !== '') {
        this.isErrorProduct = false
        this.isVisibleProduct = false
        this.isValidProduct = true
      } else {
        this.isErrorProduct = true
        this.isVisibleProduct = true
        this.isValidProduct = false
      }
    },
    linkBlur () {
      if (this.link !== '') {
        this.isErrorLink = false
        this.isVisibleLink = false
        this.isValidLink = true
      } else {
        this.isErrorLink = true
        this.isVisibleLink = true
        this.isValidLink = false
      }
    },
    priceBlur () {
      if (this.price !== '') {
        this.isErrorPrice = false
        this.isVisiblePrice = false
        this.isValidPrice = true
        this.price = this.price.replace(/(\d{1,3}(?=(?:\d\d\d)+(?!\d)))/g, '$1' + ' ')
      } else {
        this.isErrorPrice = true
        this.isVisiblePrice = true
        this.isValidPrice = false
      }
    },
    randomIndex () {
      return Math.floor(Math.random() * (10000 - 10)) + 10
    },
    onSubmit () {
      this.arrayProducts.push({ id: this.randomIndex(), imageData: `${this.link}`, titleData: `${this.product}`, descriptionData: `${this.description}`, priceData: `${this.price}`, realPrice: `${this.price.replace(/\s+/g, '').trim()}` })
      this.saveArray()
      this.link = ''
      this.product = ''
      this.description = ''
      this.price = ''
      this.isValidProduct = false
      this.isValidLink = false
      this.isValidPrice = false
      this.arrayProducts.splice(this.randomIndex(), 0)
    },
    saveArray () {
      const parsed = JSON.stringify(this.arrayProducts)
      localStorage.setItem('arrayProducts', parsed)
      this.arrayView = JSON.parse(localStorage.getItem('arrayProducts'))
    },
    removeArray (item) {
      this.arrayProducts.splice(item, 1)
      this.saveArray()
    },
    sortArray () {
      if (this.select === 'Цена по возврастанию') {
        return this.arrayView.sort(function (d1, d2) { return (+d1.realPrice > +d2.realPrice) ? 1 : -1 })
      } else if (this.select === 'Цена по убыванию') {
        return this.arrayView.sort(function (d1, d2) { return (+d1.realPrice < +d2.realPrice) ? 1 : -1 })
      } else if (this.select === 'По умолчанию') {
        return (this.arrayView = JSON.parse(localStorage.getItem('arrayProducts')))
      }
    }
  }
}
</script>

<style lang="scss">
.root {
  padding: 31px;
}
.up__menu {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
  &-title {
    font-family: Source Sans Pro;
    font-style: normal;
    font-weight: 600;
    font-size: 28px;
    line-height: 35px;
    color: #3f3f3f;
  }
  &-select {
    width: 100%;
    max-width: 160px;
    padding: 10px 0 10px 16px;
    border: 1px solid #fffefa;
    border-radius: 4px;
    font-family: Source Sans Pro;
    font-weight: normal;
    font-size: 12px;
    line-height: 15px;
    color: #B4B4B4;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    cursor: pointer;
  }
}
.main {
  display: flex;
}
.main {
  &__add-product {
    width: 100%;
    max-width: 332px;
    height: 440px;
    margin-right: 16px;
    padding: 24px;
    border: 1px solid #fffefa;
    border-radius: 4px;
    background: #FFFEFB;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    &-label {
      position: relative;
      margin-bottom: 4px;
      font-family: Source Sans Pro;
      font-weight: normal;
      font-size: 10px;
      line-height: 13px;
      color: #49485E;
      &-span {
        display: block;
        margin-bottom: 4px;
      }
    }
    &-input {
      width: 100%;
      max-width: 284px;
      margin-bottom: 15px;
      padding: 10px 0 10px 16px;
      border: 1px solid #fffefa;
      border-radius: 4px;
      font-family: Source Sans Pro;
      font-weight: normal;
      font-size: 12px;
      line-height: 15px;
      color: #B4B4B4;
      background: #FFFEFB;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    }
    &-input[name="price"] {
      margin-bottom: 24px;
    }
    &-textarea {
      width: 100%;
      max-width: 284px;
      height: 108px;
      padding: 10px 16px;
      border: 1px solid #fffefa;
      border-radius: 4px;
      font-family: Source Sans Pro;
      font-weight: normal;
      font-size: 12px;
      line-height: 15px;
      color: #B4B4B4;
      background: #FFFEFB;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    }
    &-btn {
      width: 100%;
      max-width: 284px;
      padding: 10px 0;
      border: 2px solid #7BAE73;
      border-radius: 10px;
      font-family: Inter;
      font-weight: 600;
      font-size: 12px;
      line-height: 15px;
      text-align: center;
      color: #FFFFFF;
      background: #7BAE73;
      cursor: pointer;
    }
    &-btn:disabled {
      border: 2px solid #EEEEEE;
      color: #B4B4B4;
      background: #EEEEEE;
    }
    &-btn:hover:not(:disabled) {
      background: #73AE2C;
      border: 2px solid #7BAE73;
    }
    &-btn:active:not(:disabled) {
      background: #B7F46E;
      border: 2px solid #7BAE73;
    }
    &-star {
      color: #FF8484;
    }
    &-error {
      position: absolute;
      top: 55px;
      left: 0;
      width: 100px;
      font-family: Source Sans Pro;
      font-weight: normal;
      font-size: 8px;
      line-height: 10px;
      color: #FF8484;
    }
    &-error[name="link"] {
      top: 70px;
      left: -277px;
    }
    &-error[name="price"] {
      top: 97px;
      left: -277px;
    }
  }
  &__show-product > div {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin: auto;
  }
  &__show-product {
    width: 100%;
    margin: 0 auto;
    &-wrapper {
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      flex: 0 1 32%;
      width: 100%;
      margin-bottom: 16px;
      border: 1px solid #fffefa;
      border-radius: 4px;
      background: #FFFEFB;
      box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
      cursor: pointer;
    }
    &-img {
      width: 100%;
      height: 200px;
      border-radius: 4px 4px 0 0;
      object-fit: cover;
    }
    &-info {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 100%;
      padding: 16px 16px 24px;
      &-title {
        margin-bottom: 16px;
        font-family: Source Sans Pro;
        font-weight: 600;
        font-size: 20px;
        line-height: 25px;
        color: #3F3F3F;
      }
      &-description {
        margin-bottom: 32px;
        font-family: Source Sans Pro;
        font-weight: normal;
        font-size: 16px;
        line-height: 20px;
        color: #3F3F3F;
      }
      &-price,
      &-price span {
        font-family: Source Sans Pro;
        font-style: normal;
        font-weight: 600;
        font-size: 24px;
        line-height: 30px;
        color: #3F3F3F;
      }
    }
    &-remove {
      position: absolute;
      top: -15px;
      right: -15px;
      opacity: .7;
    }
    &-remove:hover {
      opacity: 1;
    }
    &-remove:active {
      opacity: .5;
    }
  }
}
.error {
  border: 1px solid #FF8484;
}
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-move {
  transition: transform 1s;
}

@media (max-width: 1023px) {
  .main {
    &__show-product {
      &-wrapper {
        flex: 0 1 45%;
      }
    }
  }
}

@media (max-width: 850px) {
  .up__menu {
    &-select {
      position: absolute;
      top: 534px;
      right: 0;
      left: 0;
      margin: auto;
    }
    &-title {
      margin: auto;
    }
  }
  .main {
    display: flex;
    flex-direction: column;
    &__add-product {
      margin: auto;
      margin-bottom: 70px;
      padding: 24px;
    }
    &__show-product {
      &-wrapper {
        flex: 0 1 45%;
      }
    }
  }
}

@media (max-width: 450px) {
  .root {
    padding: 15px;
  }
  .up__menu {
    &-select {
      display: none;
    }
  }
  .main {
    display: flex;
    flex-direction: column;
    &__add-product {
      margin: auto;
      margin-bottom: 30px;
      padding: 15px;
    }
    &__show-product {
      &-wrapper {
        flex: 0 1 100%;
      }
    }
  }
}
</style>
