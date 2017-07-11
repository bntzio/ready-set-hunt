<template lang="pug">
  .app__main__checklist__list__content
    input.app__main__checklist__list__content__checkbox(v-bind:id="item.id" v-bind:value="item.id" type="checkbox" v-bind:name="item.text" v-bind:checked="item.completed" @change="onchange")
    label.app__main__checklist__list__content__item(v-bind:for="item.id") {{ item.text }}
</template>

<script>
export default {
  name: 'item',
  props: [ 'item' ],
  methods: {
    onchange(ev) {
      this.$emit('checked', this.item, ev.target.checked);
    }
  }
}
</script>

<style lang="scss">
.app__main__checklist__list__content {
  margin: .3rem 1.5rem;

  &__checkbox {
    margin-right: 10px;
    width: 1.3em;
    height: 1.3em;
    background-color: white;
    border-radius: 50%;
    vertical-align: middle;
    border: 1px solid #ddd;
    -webkit-appearance: none;
    outline: none;
    cursor: pointer;

    &:checked {
      border: none;
      background: #E4984A;
    }
  }

  &__item {}
}
</style>
