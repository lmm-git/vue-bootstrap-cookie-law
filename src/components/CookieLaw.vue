<template>
  <transition appear :name="transitionName">
    <div class="cookie cookie-notice" :class="[containerPosition, cookieTheme]" v-if="isOpen">
      <div class="cookie-content">
        <slot name="message"><p>{{ message }}</p></slot>
      </div>
      <b-button-group class="cookie-buttons">
        <b-button class="cookie-button" v-if="informationButtonLink" :variant="informationButtonVariant" :to="informationButtonLink">{{ informationButtonText }}</b-button>
        <b-button class="cookie-button" :variant="closeButtonVariant" @click="accept">{{ closeButtonText }}</b-button>
      </b-button-group>
    </div>
  </transition>
</template>

<script>
  import * as Cookie from 'tiny-cookie'
  export default {
    props: {
      closeButtonText: {
        type: String,
        default: 'Got it!'
      },
      informationButtonLink: {
        type: [String, Object],
        required: false
      },
      informationButtonText: {
        type: String,
        default: 'More info'
      },
      message: {
        type: String,
        default: 'This website uses cookies to ensure you get the best experience on our website.'
      },
      theme: {
        type: String,
        default: 'base'
      },
      /**
       * Cookie Container position
       * bottom, top
       * @type {Object}
       */
      position: {
        type: String,
        default: 'bottom'
      },
      /**
       * Transition name has following possibilities
       * slideFromBottom
       * slideFromTop
       * fade
       * @type {Object}
       */
      transitionName: {
        type: String,
        default: 'slideFromBottom'
      },
      closeButtonVariant: {
        type: String,
        default: 'primary'
      },
      informationButtonVariant: {
        type: String,
        default: 'outline-primary'
      }
    },
    data () {
      return {
        supportsLocalStorage: true,
        isOpen: false
      }
    },
    computed: {
      containerPosition () {
        return `cookie-${this.position}`
      },
      cookieTheme () {
        return `cookie-${this.theme}`
      },
      externalButtonLink () {
        return typeof this.buttonLink === 'string' && this.buttonLink.length
      },
      internalButtonLink () {
        return typeof this.buttonLink === 'object' && this.buttonLink != null && Object.keys(this.buttonLink).length
      },
      target () {
        return this.buttonLinkNewTab ? '_blank' : '_self'
      }
    },
    created () {
      // Check for availability of localStorage
      try {
        const test = '__vue-cookielaw-check-localStorage'

        window.localStorage.setItem(test, test)
        window.localStorage.removeItem(test)
      } catch (e) {
        console.error('Local storage is not supported, falling back to cookie use')
        this.supportsLocalStorage = false
      }

      if (!this.getVisited() === true) {
        this.isOpen = true
      }
    },
    methods: {
      setVisited () {
        if (this.supportsLocalStorage) {
          localStorage.setItem('cookie:accepted', true)
        } else {
          Cookie.set('cookie:accepted', true)
        }
      },
      getVisited () {
        if (this.supportsLocalStorage) {
          return localStorage.getItem('cookie:accepted')
        } else {
          return Cookie.get('cookie:accepted')
        }
      },
      accept () {
        this.setVisited()
        this.isOpen = false
        this.$emit('accept')
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "~bootstrap/scss/bootstrap";

  .cookie {
    position: fixed;
    overflow: hidden;
    box-sizing: border-box;
    z-index: 9999;
    width: 100%;
    justify-content: space-between;
    align-items: baseline;
    flex-direction: column;

    @include media-breakpoint-up('sm') {
      flex-flow: row;
      display: flex;

      .cookie-buttons {
        margin-left: 15px;
      }
    }

    @include media-breakpoint-down('sm') {
      text-align: center;

      .btn-group {
        display: inline-flex;
      }
    }

    padding: 8px 15px 8px 15px;
  }

  .cookie-top {
    top: 0;
    left: 0;
    right: 0;
  }

  .cookie-bottom {
    bottom: 0;
    left: 0;
    right: 0;
  }
  .cookie-buttons {
    display: flex;
    flex-direction: row;

    > * {
      margin: rem(5) 0;
    }
  }

  .cookie-content {
    p {
      margin: 0;
    }
  }

  .slideFromTop-enter, .slideFromTop-leave-to {
    transform: translate(0px, -12.500em);
  }

  .slideFromTop-enter-to, .slideFromTop-leave {
    transform: translate(0px, 0px);
  }

  .slideFromBottom-enter, .slideFromBottom-leave-to {
    transform: translate(0px, 12.500em);
  }

  .slideFromBottom-enter-to, .slideFromBottom-leave {
    transform: translate(0px, 0px);
  }

  .slideFromBottom-enter-active,
  .slideFromBottom-leave-active,
  .slideFromTop-enter-active,
  .slideFromTop-leave-active, {
    transition: transform .4s ease-in;
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }


  @mixin generateTheme($theme, $backgroundColor, $opacity: 1) {
    .cookie-#{$theme} {
      background: $backgroundColor;
      opacity: $opacity;
    }
  }

  @include generateTheme('base', #F1F1F1);
  @include generateTheme('gray-transparent', #F1F1F1, 0.85);
</style>
