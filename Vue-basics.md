# Vue Basics

- [Vue Basics](#vue-basics)
  - [Defining a component](#defining-a-component)
  - [Using a component](#using-a-component)
  - [Passing Props](#passing-props)
    - [Web explanation](#web-explanation)
    - [GPT explanation](#gpt-explanation)
  - [Event Emitting / Event listeners (passing info back to parent)](#event-emitting--event-listeners-passing-info-back-to-parent)
    - [Final Code Example](#final-code-example)
  - [](#)


Using [This tutorial](https://vuejs.org/guide/essentials/component-basics.html)

## Defining a component

When using a build step, we typically define each Vue component in a dedicated file using the .vue extension - known as a Single-File Component (SFC for short):

```js

<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```

When not using a build step, a Vue component can be defined as a plain JavaScript object containing Vue-specific options:

```js

import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return { count }
  },
  template: `
    <button @click="count++">
      You clicked me {{ count }} times.
    </button>`
  // Can also target an in-DOM template:
  // template: '#my-template-element'
}
```

The template is inlined as a JavaScript string here, which Vue will compile on the fly. You can also use an ID selector pointing to an element (usually native <template> elements) - Vue will use its content as the template source.

The example above defines a single component and exports it as the default export of a .js file, but you can use named exports to export multiple components from the same file.

## Using a component

To use a child component, we need to import it in the parent component. Assuming we placed our counter component inside a file called ButtonCounter.vue, the component will be exposed as the file's default export:
vue

```js
<script setup>
import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```

With <script setup>, imported components are automatically made available to the template.

It's also possible to globally register a component, making it available to all components in a given app without having to import it. The pros and cons of global vs. local registration is discussed in the dedicated Component Registration section.

Components can be reused as many times as you want:
template

```html
<h1>Here are many child components!</h1>
<ButtonCounter />
<ButtonCounter />
<ButtonCounter />
```

Notice that when clicking on the buttons, each one maintains its own, separate count. That's because each time you use a component, a new instance of it is created.

In SFCs, it's recommended to use PascalCase tag names for child components to differentiate from native HTML elements. Although native HTML tag names are case-insensitive, Vue SFC is a compiled format so we are able to use case-sensitive tag names in it. We are also able to use /> to close a tag.

If you are authoring your templates directly in a DOM (e.g. as the content of a native <template> element), the template will be subject to the browser's native HTML parsing behavior. In such cases, you will need to use kebab-case and explicit closing tags for components:

```html
<!-- if this template is written in the DOM -->
<button-counter></button-counter>
<button-counter></button-counter>
<button-counter></button-counter>
```

See in-DOM template parsing caveats for more details.

## Passing Props 

### Web explanation

Props are custom attributes you can register on a component. To pass a title to our blog post component, we must declare it in the list of props this component accepts, using the defineProps macro:
vue

```js
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```

defineProps is a compile-time macro that is only available inside <script setup> and does not need to be explicitly imported. Declared props are automatically exposed to the template. defineProps also returns an object that contains all the props passed to the component, so that we can access them in JavaScript if needed:

```js
const props = defineProps(['title'])
console.log(props.title)
```

If you are not using <script setup>, props should be declared using the props option, and the props object will be passed to setup() as the first argument:

```js

export default {
  props: ['title'],
  setup(props) {
    console.log(props.title)
  }
}
```

A component can have as many props as you like and, by default, any value can be passed to any prop.

Once a prop is registered, you can pass data to it as a custom attribute, like this:
template

```js
<BlogPost title="My journey with Vue" />
<BlogPost title="Blogging with Vue" />
<BlogPost title="Why Vue is so fun" />
```

In a typical app, however, you'll likely have an array of posts in your parent component:

```js

const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])
```

Then want to render a component for each one, using v-for:
template

```js
<BlogPost
  v-for="post in posts"
  :key="post.id"
  :title="post.title"
 />
```
### GPT explanation

Passing props in Vue.js is a fundamental concept that allows you to pass data from a parent component to a child component. Props are custom attributes you can register on a component. When you pass props to a child component, the child component can then use that data internally.

**How to Pass Props to Vue Components**

1. **Define Props in the Child Component**:
   In the child component, you define the props that you expect to receive using the `defineProps` macro (for Vue 3 with `<script setup>`) or the `props` option (for regular Vue 2 or 3 components).

2. **Pass Props from the Parent Component**:
   In the parent component, you pass props to the child component by adding attributes to the child component’s tag.

**Example with a Child Component Receiving 3 Different Props**

Let's create a simple example where a child component receives three different props: `title`, `content`, and `author`.

**Step 1: Create the Child Component `BlogPost.vue`**

```vue
<!-- BlogPost.vue -->
<script setup>
defineProps({
  title: String,
  content: String,
  author: String,
});
</script>

<template>
  <div class="blog-post">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
    <small>Written by: {{ author }}</small>
  </div>
</template>

<style scoped>
.blog-post {
  border: 1px solid #ddd;
  padding: 16px;
  margin-bottom: 16px;
}
</style>
```

In this component:
- We use the `defineProps` macro to define the three props: `title`, `content`, and `author`. These props are expected to be of type `String`.
- Inside the template, we use these props to display the blog post’s title, content, and author.

**Step 2: Use the Child Component in the Parent Component `BlogParent.vue`**

```vue
<!-- BlogParent.vue -->
<script setup>
import { ref } from 'vue';
import BlogPost from './BlogPost.vue';

const posts = ref([
  { id: 1, title: 'My Journey with Vue', content: 'Vue.js is an amazing framework...', author: 'Alice' },
  { id: 2, title: 'Why I Love JavaScript', content: 'JavaScript allows me to build...', author: 'Bob' },
  { id: 3, title: 'The Future of Web Development', content: 'Web development is evolving...', author: 'Charlie' },
]);
</script>

<template>
  <div>
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :content="post.content"
      :author="post.author"
    />
  </div>
</template>
```

In the parent component:
- We import the `BlogPost` component.
- We define an array of `posts`, each containing `id`, `title`, `content`, and `author` properties.
- We use a `v-for` loop to iterate over each `post` and pass the `title`, `content`, and `author` props to the `BlogPost` component using the `v-bind` shorthand `:`.

**Explanation**

- **Props Definition**: In `BlogPost.vue`, `defineProps` is used to declare which props the component expects. This makes it clear and helps with type-checking.
- **Passing Props**: In `BlogParent.vue`, props are passed using the `:` shorthand syntax (`:title="post.title"`). This binds the parent’s data to the child’s props.
- **Using Props**: The child component (`BlogPost.vue`) uses the props within its template to display the data.

**Summary**

- **Child Component**: Define the props you expect with `defineProps` or `props`.
- **Parent Component**: Pass data to the child by binding props.
- **Reactivity**: Props are reactive, meaning if the parent’s data changes, the child component will update automatically.

## Event Emitting / Event listeners (passing info back to parent)

The child components can communicate back up to their parent components using custom events. This is particularly useful when a child component needs to trigger a change in the parent component or inform it about some action, like a button click.

1. **Setting Up the Parent Component with Reactive Data**

In your `BlogParent.vue`, you already have a `posts` array defined as a `ref`, which is reactive and can be iterated over to display individual `BlogPost` components. The tutorial adds another reactive variable called `postFontSize` to control the font size of all blog posts:

```javascript
const postFontSize = ref(1);
```

- **`postFontSize`**: This variable starts with a value of `1` (em units). It's used to control the font size of all blog posts collectively.

In the parent template, the font size is applied to the entire `div` that wraps the `BlogPost` components:

```vue
<template>
  <div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :content="post.content"
      :author="post.author"
    />
  </div>
</template>
```

- **`v-for` and `:style`**: Here, the `v-for` loop iterates over the `posts` array, passing each post's data as props to the `BlogPost` component. The `:style` directive dynamically binds the `postFontSize` to the `font-size` CSS property, affecting the size of all text inside this `div`.

1. **Adding an "Enlarge Text" Button in the Child Component**

Now, to control the font size from within each `BlogPost`, you add a button to the `BlogPost.vue` template:

```vue
<template>
  <div class="blog-post">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
    <small>Written by: {{ author }}</small>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```

- **Button and `$emit`**: The button's `@click` event handler uses `$emit('enlarge-text')` to emit a custom event called `enlarge-text` whenever the button is clicked.

3. **Listening to the Custom Event in the Parent Component**

In the parent component, you need to listen for this `enlarge-text` event. This is done by adding an event listener using the `@` directive in the `BlogParent.vue`:

```vue
<template>
  <div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :content="post.content"
      :author="post.author"
      @enlarge-text="postFontSize += 0.1"
    />
  </div>
</template>
```

- **Event Listener**: The `@enlarge-text="postFontSize += 0.1"` listens for the `enlarge-text` event emitted by any of the `BlogPost` components. When the event is triggered, it increases the `postFontSize` by `0.1em`.

**Putting It All Together**

This is how the tutorial implements communication between a parent and a child component using props and custom events:

1. **Props**: Data is passed from the parent (`BlogParent.vue`) to the child (`BlogPost.vue`) using props like `title`, `content`, and `author`.

2. **Custom Events**: The child component (`BlogPost.vue`) can communicate back to the parent by emitting events, such as `enlarge-text`.

3. **Parent's Response**: The parent listens to these events and reacts by modifying its own state or performing some action—in this case, adjusting the font size.

### Final Code Example

**Parent Component: BlogParent.vue**
```vue
<script setup>
import { ref } from 'vue';
import BlogPost from './BlogPost.vue';

const posts = ref([
  { id: 1, title: 'My Journey with Vue', content: 'Vue.js is an amazing framework...', author: 'Alice' },
  { id: 2, title: 'Why I Love JavaScript', content: 'JavaScript allows me to build...', author: 'Bob' },
  { id: 3, title: 'The Future of Web Development', content: 'Web development is evolving...', author: 'Charlie' },
]);

const postFontSize = ref(1);
</script>

<template>
  <div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :content="post.content"
      :author="post.author"
      @enlarge-text="postFontSize += 0.1"
    />
  </div>
</template>
```

**Child Component: BlogPost.vue**
```vue
<script setup>
defineProps({
  title: String,
  content: String,
  author: String,
});
</script>

<template>
  <div class="blog-post">
    <h2>{{ title }}</h2>
    <p>{{ content }}</p>
    <small>Written by: {{ author }}</small>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>

<style scoped>
.blog-post {
  border: 1px solid #ddd;
  padding: 16px;
  margin-bottom: 16px;
}
</style>
```

## 