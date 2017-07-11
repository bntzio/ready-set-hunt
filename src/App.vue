<template lang="pug">
  .app
    .app__header
      .app__header__logo
        h1.app__header__logo__title Ready Set Hunt
        h2.app__header__logo__subtitle The Ultimate Product Hunt Launch Checklist
      span.app__header__brought Brought to you by @bntzio
    .app__main
      .app__main__checklist
        .app__main__checklist__top
          form.app__main__checklist__top__form(v-on:submit.prevent="submitName")
            input.app__main__checklist__top__form__product(type="text" v-model="productName" v-bind:value="productName" placeholder="Product Name")
            button.app__main__checklist__top__form__button(type="submit") OK
          .app__main__checklist__top__progress
        div.app__main__checklist__list
          item(v-for="item in checklist" v-bind:item="item" v-bind:key="item.id" @checked="setItem")
</template>

<script>
import Item from './components/Item.vue';
import checklist from './data/';

export default {
  name: 'app',
  data () {
    return {
      productName: '',
      checklist: this.getChecklist(checklist)
    }
  },
  components: {
    Item
  },
  methods: {
    submitName() {
      window.localStorage.setItem('productName', JSON.stringify(this.productName));
    },
    getProductName() {
      return JSON.parse(window.localStorage.getItem('productName'));
    },
    getCheckedItems() {
      return JSON.parse(window.localStorage.getItem('items'));
    },
    setItem(item, checked) {
      if (checked) {
        item.completed = true;
        let items = this.getCheckedItems() || [];
        items.push(item);
        window.localStorage.setItem('items', JSON.stringify(items));
      } else {
        const items = this.getCheckedItems();
        if (items) {
          const itemToRemove = item.id;
          let updatedItems = [];
          items.forEach(function(item) {
            if (item.id !== itemToRemove) {
              updatedItems.push(item);
            }
          });
          window.localStorage.setItem('items', JSON.stringify(updatedItems));
        }
      }
    },
    getChecklist(originalChecklist) {
      const storedChecklist = this.getCheckedItems();
      if (storedChecklist) {
        let storedIds = [];
        storedChecklist.forEach(item => {
          storedIds.push(item.id);
        });
        let newChecklist = [];
        originalChecklist.forEach(item => {
          if (storedIds.includes(item.id)) {
            item.completed = true;
            newChecklist.push(item);
          } else {
            newChecklist.push(item);
          }
        });
        return newChecklist;
      }
      return originalChecklist;
    }
  },
  mounted() {
    this.productName = this.getProductName() || '';
  }
}
</script>

<style lang="scss">
html {
  height: 100%;
}

body {
  height: 100%;
  margin: 0;
  background: #E4984A;
  background: linear-gradient(45deg, #E4984A, #BB4269);
  background-repeat: no-repeat;
  background-attachment: fixed;
  color: white;
  font-size: 16px;
}

.app {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;

  &__header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 1rem 3rem;

    @media(min-width: 768px) {
      justify-content: space-between;
    }

    &__logo {
      display: flex;
      flex-direction: column;
      align-items: center;

      &__title {
        font-size: 2rem;
        font-weight: 900;
        text-transform: uppercase;
        font-family: 'Zooja W00 Regular';
        letter-spacing: 2px;
        margin-bottom: 0;
      }

      &__subtitle {
        font-size: .7rem;
        margin-top: 0;
        font-family: 'KievitWeb W03 MediIta';
      }
    }

    &__brought {
      display: none;
      font-family: 'Proxima N W01 Smbd It';
      font-size: .8rem;

      @media(min-width: 768px) {
        display: block;
      }
    }
  }

  &__main {
    display: flex;
    justify-content: center;
    color: #333;
    height: 100%;

    &__checklist {
      position: relative;
      padding: 2rem 10rem;
      margin-top: 3.4rem;
      background: white;
      border-radius: 2.7px;
      box-shadow: 10px 10px 45px -3px rgba(0,0,0,0.6);
      font-family: 'Proxima N W01 Reg';

      @media(min-width: 768px) {
        padding: 2rem 15rem;
      }

      &__top {
        background: peru;
        position: absolute;
        width: 100%;
        top: 0;
        left: 0;
        padding: 1rem 0;

        &__form {
          display: flex;
          justify-content: space-between;

          &__product {
            position: absolute;
            top: 0;
            height: 100%;
            width: 80%;
            border: none;
            padding: 3px;
            padding-left: 1rem;
            font-size: .8rem;
            border-radius: 2.7px;

            &:focus {
              outline-width: 0;
            }
          }

          &__button {
            position: absolute;
            top: 0;
            right: 0;
            height: 2.37rem;
            width: 20%;
            border: none;
            outline: none;
            background: #E4984A;
            background: linear-gradient(45deg, #E4984A, #BB4269);
            color: white;
            font-weight: 900;
            cursor: pointer;
          }
        }

        &__progress {
          position: absolute;
          bottom: -10px;
          height: 5px;
          background: #E4984A;
          background: linear-gradient(50deg, #E4984A, #BB4269);
          width: 100%;
        }
      }

      &__list {
        position: absolute;
        top: 10px;
        left: 0;
        margin-top: 3rem;
      }
    }
  }
}
</style>
