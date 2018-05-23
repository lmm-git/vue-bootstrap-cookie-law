# 🍪 👮 Vue Cookie Law
[![Build Status](https://travis-ci.org/lmm-git/vue-bootstrap-cookie-law.svg?branch=master)](https://travis-ci.org/lmm-git/vue-bootstrap-cookie-law)
[![npm](https://img.shields.io/npm/v/vue-bootstrap-cookie-law.svg)](https://www.npmjs.com/package/vue-bootstrap-cookie-law)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/apertureless/vue-cookie-law/blob/master/LICENSE)

EU Cookie Law Plugin for Vue.js - Originally written by Jakub Juszczak <jakub@posteo.de>

## 🔧  Install
`yarn add vue-bootstrap-cookie-law`

## 👈 Usage

```javascript

<template>
  <footer>
    <cookie-law theme="white-transparent"></cookie-law>
  </footer>
</template>

<script>
  import CookieLaw from 'vue-bootstrap-cookie-law'
  export default {
    components: { CookieLaw }
  }
</script>
```

## Slots

You can also pass in the message into a named slot. This way you can for example add `<router-link>` and other dynamic content.

```html
<cookie-law>
  <div slot="message">
    <p>Here is my message for more info <router-link to="legal-notes">Click here</router-link></p>
  </div>
</cookie-law>
```

## Props
| prop | default | type | description
|---|---|---|---|
| buttonText | 'Got It!' | String | 🔘 Well, its the button text
| buttonLink |  | String\|Object | Link to more infos. Simple href or a [vue-router](https://github.com/vuejs/vue-router) Location object
| buttonLinkText | 'More info' | String | Label of link button
| buttonLinkNewTab | false | Boolean | If true, it opens the link in a new tab/window (href)
| buttonClass | 'Cookie__button' | String | Custom class name for buttons
| message | 'This website uses cookies to ensure you get the best experience on our website.' | String | Your message in the content area
| theme | 'base' | String | Selected theme. You can also create a custom one
| position | 'bottom' | String | Possible positions are `bottom` or `top`
| transitionName | 'slideFromBottom' | String | Enter and leave transitions. Currenty supported `slideFromBottom`, `slideFromTop`, `fade`

## Events

The default button will emit an `accept` event you can listen on if the user clicks the button.

```html
<cookie-law v-on:accept="ThankYouMethod()"/>
```

## 💅 Themes

![Cookie Law Themes](static/cookie-law-themes.png)

### Custom Themes
You can easy create your own themes. The classes that need to be styled are:

- `.cookie` for the container
- `.cookie-content` for the content with message
- `.cookie-button` for the button

And then pass your theme name to the component.

## :copyright: License

[MIT](http://opensource.org/licenses/MIT)
