<template lang="pug">
  .app
    .app__header
      h1.app__header__title Ready Set Hunt
      span.app__header__brought Brought to you by @bntzio
    .app__main
      .app__main__checklist
        .app__main__checklist__top
          form.app__main__checklist__top__form(v-on:submit.prevent="submitName")
            input.app__main__checklist__top__product(type="text" v-model="productName" v-bind:value="productName" placeholder="Product Name")
            button.app__main__checklist__top__button(type="submit") OK
          .app__main__checklist__top__progress
        ul.app__main__checklist__list(v-for="item in checklist")
          li.app__main__checklist__list__item {{ item.text }}
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      productName: '',
      checklist: [
        {
          id: 1,
          text: 'Sign up on Product Hunt',
          completed: false
        },
        {
          id: 2,
          text: 'Get an invitation',
          completed: false
        },
        {
          id: 3,
          text: 'Finish your product',
          completed: false
        },
        {
          id: 4,
          text: 'Launch your product',
          completed: false
        }
      ]
    }
  },
  methods: {
    submitName() {
      console.log(this.productName);
    }
  }
}
</script>

<style lang="scss">
// styles here!! 🖌️
</style>
