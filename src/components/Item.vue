<template lang="pug">
  .app__main__checklist__list__content(v-model="item")
    input.app__main__checklist__list__content__checkbox(v-bind:id="item.id" v-bind:value="item.id" type="checkbox" v-bind:name="item.text" v-bind:checked="item.completed" @change="onchange")
    label.app__main__checklist__list__content__item(v-bind:for="item.id")
      span
      i.app__main__checklist__list__content__item__text(v-bind:class="{ marked: item.completed === true }")
        | {{ item.text }}
</template>

<script>
export default {
  name: 'item',
  props: [ 'item' ],
  methods: {
    onchange(ev) {
      this.$emit('checked', this.item, ev.target.checked);
      if (ev.target.checked) {
        document.getElementsByClassName('app__main__checklist__list__content')[this.item.id - 1].getElementsByTagName('i')[0].classList.add('marked');
      } else {
        document.getElementsByClassName('app__main__checklist__list__content')[this.item.id - 1].getElementsByTagName('i')[0].classList.remove('marked');
      }
    }
  }
}
</script>

<style lang="scss">
.app__main__checklist__list__content {
  margin: .3rem 1.5rem;

  &__checkbox {}

  &__item {}

  label {
    display: inline-block; // to make it easier to click
    cursor: pointer;
    position: relative; // important

    // Now we'll create the checkbox object
    span {
      display: inline-block;
      position: relative;
      background-color: transparent;
      width: 17px;
      height: 17px;
      transform-origin: center;
      border: 2px solid #E4984A;
      border-radius: 50%;
      vertical-align: -6px;
      margin-right: 10px;
      transition: transform 350ms cubic-bezier(.78,-1.22,.17,1.89); // custom ease effect for bouncy animation

      // Now we'll create the "tick" using pseudo elements - those will be basically two lines that will be rotated to form the "tick"
      &:before {
        content: "";
        width: 0px;
        height: 2px;
        border-radius: 2px; // so that the tick has nice rounded look
        background: #E4984A;
        position: absolute;
        transform: rotate(45deg);
        top: 8px; // you'll need to experiment with placement depending on the dimensions you've chosen
        left: 5px; // you'll need to experiment with placement depending on the dimensions you've chosen
        transition: width 50ms ease 50ms;
        transform-origin: 0% 0%;
      }

      &:after {
        content: "";
        width: 0;
        height: 2px;
        border-radius: 2px; // so that the tick has nice rounded look
        background: #E4984A;
        position: absolute;
        transform: rotate(305deg);
        top: 12px; // you'll need to experiment with placement depending on the dimensions you've chosen
        left: 6px; // you'll need to experiment with placement depending on the dimensions you've chosen
        transition: width 50ms ease;
        transform-origin: 0% 0%;
      }

      &:hover {
        border-color: #E18B34;
      }
    }

    // Text next to the checkbox
    i {
      font-style: normal;
    }

    // Time to add some life to it
    &:hover {
      span {
        &:before {
          width: 5px;
          transition: width 100ms ease;
        }

        &:after {
          width: 10px;
          transition: width 150ms ease 100ms;
        }
      }
    }
  }

  input[type="checkbox"] {
    display: none; // hide the system checkbox

    // Let's add some effects after the checkbox is checked
    &:checked {
      + label {
        span {
          background-color: #E4984A;
          border-color: #E4984A;
          transform: scale(1.15); // enlarge the box

          &:after {
            width: 10px;
            background: white;
            transition: width 150ms ease 100ms; // enlarge the tick
          }

          &:before {
            width: 5px;
            background: white;
            transition: width 150ms ease 100ms; // enlarge the tick
          }
        }

        &:hover { // copy the states for onMouseOver to avoid flickering
          span {
            background-color: #E18B34;
            border-color: #E18B34;
            transform: scale(1.15); // enlarge the box

            &:after {
              width: 10px;
              background: white;
              transition: width 150ms ease 100ms; // enlarge the tick
            }

            &:before {
              width: 5px;
              background: white;
              transition: width 150ms ease 100ms; // enlarge the tick
            }
          }
        }
      }
    }
  }

  .marked {
    line-height: 1em;
    position: relative;
    color: #333;
    font-style: italic;
    opacity: .2;

    &:after {
      border-bottom: 0.125em solid #333;
      content: "";
      left: 0;
      margin-top: calc(0.125em / 2 * -1);
      position: absolute;
      right: 0;
      top: 50%;
    }
  }
}
</style>
