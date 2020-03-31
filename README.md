# Blibli UI Dev Guideline

Standardization for developing pyeongyang UI with reasonable performance approach


## Getting Started
Before Continue to the content make sure you already read and understand this following concept or tutorials

[Block Element Modifier](http://getbem.com/)

[Airbnb Javascript Style Guide](https://github.com/airbnb/javascript)

[VueJs Style Guide](https://vuejs.org/v2/style-guide/)


## Table of Contents

1. Common
2. CSS 
3. VueJs
4. Commit Message standardisation

## Common

### Don't go beyond 80 cols both in .vue and .js files
```vue
//bad approach

<template>
  <div>
    <div class="header"><div class="header__left">Logo</div><div class="header__right">User</div></div>
  </div>
</template>

//good approach

<template>
  <div>
    <div class="header">
      <div class="header__left">Logo</div>
      <div class="header__right">User</div>
    </div>
  </div>
</template>
```
```js

//bad approach

export default {
  name: 'test',
  props: {
    students: {
      type: Array,
      default: []
    }
  },
  computed: {
    goodStudents () {
      return this.students.length > 0 && this.students.filter(student => { return student.score > 7 && student.extracurricular > 2})
    }
  }
}

//good approach

export default {
  name: 'test',
  props: {
    students: {
      type: Array,
      default: []
    }
  },
  computed: {
    goodStudents () {
      return this.students.length > 0 && this.students.filter(student => { 
        return student.score > 7 && student.extracurricular > 2
      })
    }
  }
}

//best approach

export default {
  name: 'test',
  props: {
    students: {
      type: Array,
      default: []
    }
  },
  computed: {
    goodStudents () {
      return this.students.length > 0 &&
        this.students.filter(student => { 
        return student.score > 7 &&
          student.extracurricular > 2
      })
    }
  }
}

```

### Use if shorthand syntax

```js
//bad approach
computed: {
    goodStudents () {
      if(this.student.score > 7){
        return 'I am the good guy'
      } else {
        return 'I am the bad guy, duh '
      }
    }
}

//good approach
computed: {
    goodStudents () {
      if(this.student.score > 7) return 'I am the good guy'
      else return 'I am the bad guy, duh '
    }
}

//best approach
computed: {
    goodStudents () {
      return this.student.score > 7 ? 'I am the good guy': 'I am the bad guy, duh '
    }
}
```

### Copied array or object in javascript
```js
//bad approach 

const variable1 = {name : 'cat'}
const variable2 = variable1

const variable3 = ['lucky', 'cat']
const variable5 = variable3


//good approach (So last year)

const variable1 = {name : 'cat'}
const variable2 = Object.assign({}, variable1)

const variable3 = ['lucky', 'cat']
const variable5 = variable3.slice(0)


//best approach (Wow So trendy)

const variable1 = {name : 'cat'}
const variable2 = {...variable1}

const variable3 = ['lucky', 'cat']
const variable5 = [...variable3]

```

### Don't forget to compress image before copied to project folder

Reduce image until it's very small with tinypng.com
Reduce svg size with https://jakearchibald.github.io/svgomg/ with compression level 2

## CSS

In Pyeongyang UI we are agreed to use scss
```scss

// bad approach

.header {}
.header__left {}
.header__middle {}
.header__middle__search {}
.header__right {}

// bad approach

.header {}
.left {}
.middle {}
.middle .search {}
.right {}

// good approach

.header {
  &__left {}
  &__middle {
    &__search {}
  }
  &__right {}
}

// best approach
//break the chain if you're already inside element

.header {
   &__left {}
   &__middle {
     .search {}
   }
   &__right {}
 }

```

## Vue Js
### Vue Template
Use VueJs files template by this following order
```vue
<template>
</template>

//use external javascript from js folder
<script src="./js/${fileName}"></script>

//use scss style and scoped the style
<style lang="scss" scoped></style>

```
### Js Template
Use Javascript template by this following order. use as needed. No need create all this lifecycle Hooks!
```js
export default {
  name: 'App',
  components: {},
  props: {},
  data () {
    return {}
  },
  beforeCreate(){},
  created () {
    //Will be execute before the UI being rendered
    //Used for important API Calls
  },
  beforeMount (){},
  mounted () {
    //Will be execute After the UI being rendered
    //Used for DOM manipulation and less important API Calls
  },
  computed: {},
  methods: {},
  watch: {},
  beforeDestroy: {},
  destroyed: {}
}
```

### Import Component
Use Async import for all component (Code Splitting Approach)

[More About Code Splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting)
 ```js
const TestComponent = () => import(
  /* webpackChunkName: "c-test-component"*/ '@/components/TestComponent.vue')
export default {
  name: 'Test',
  components: {
    TestComponent
  },
}
```
### Import External Js utils or function
Import only spesific function you want to use (Tree Shaking Approach)

[More About Tree Shaking](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking)
```js
import { currencyFormat } from '@/utils'
export default {
  name: 'App',
  data() {
    return {
      price: 1000
    }
  },
  computed: {
    priceInIdr() {
      return currencyFormat(this.price)
    }
  }
}
```
### Use Lazy Load image Component from Py-Main
How To install it 
1. npm Install --save supports-webp
2. Add util with name "asset.js"

```js
import supportsWebP from 'supports-webp'

function akamaiImage (url) {
  if (!url) return
  if (!supportsWebP || url.indexOf('.gif') > -1) return url
  const prefix = url.indexOf('?') > -1 ? '&' : '?'
  return url + prefix + 'output-format=webp'
}
export { akamaiImage }
```
3. Add mixins with name "akamai-image-mixin.js"
```js
import { akamaiImage } from '@/util/asset'

export default {
  methods: {
    akamaiImage
  }
}
```
4. Add to mixins to your component Js
```js
import AkamaiImageMixin from '@/mixins/akamai-image-mixin'

mixins: [
    AkamaiImageMixin
  ]
```
5. Use It in your template vue 
```vue
<lazy-image
    class="lazy-image"
    :lazy-src="akamaiImage(test.imageUrl)"
/>
```

### Commit Message Standardisation

```
${Type} - {Subject} 

${Body}

${Reference}
```

#### Type

This should be filled by type of your task
 
- feature: add new feature
- bugfix: a bug fixing
- test: unit test improvement
- config: updating depedencies or build configuration

#### Subject

Essence description about your task

#### Body (optional)
To explain something about the task or explain how you doing this task

#### Reference (optional)
For external when doing the task

#### Example

```
feature - product review detail implementation

register new routes for review detail page,
add new pages component for review detail,
breakdown each content into smaller components (review info, review rating, review other product)
create interaction with lazy load component using intersection observer

https://app.zeplin.io/project/123123
https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
https://stackoverflow.com/questions/43663665/what-is-the-use-case-of-intersectionobserver
```

```
bugfix - fixing broken image in my review page if backend didn't return the correct image url

add blibli.com default image if the image is not found 
```

```
config - Add vue-cat-carousel and vue-h-zoom library

add vue-cat-carousel for product review page in other review component
add vue-h-zoom for my review page in zoomable review image component

https://www.npmjs.com/package/vue-cat-carousel
```
