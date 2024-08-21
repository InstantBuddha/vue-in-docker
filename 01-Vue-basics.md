# Vue Basics

- [Vue Basics](#vue-basics)
  - [Defining a component](#defining-a-component)
  - [Using a component](#using-a-component)
  - [Passing Props](#passing-props)
    - [Web explanation](#web-explanation)
    - [GPT explanation](#gpt-explanation)
  - [Event Emitting / Event listeners (passing info back to parent)](#event-emitting--event-listeners-passing-info-back-to-parent)
    - [Final Code Example](#final-code-example)
    - [Defining emitted events](#defining-emitted-events)
  - [Content Distribution with Slots](#content-distribution-with-slots)
  - [Dynamic components](#dynamic-components)
  - [ref in Vue](#ref-in-vue)
    - [1. **Using `ref` for Reactive Data**](#1-using-ref-for-reactive-data)
    - [2. **Using `ref` for DOM Elements**](#2-using-ref-for-dom-elements)
    - [3. **Accessing `ref` Values**](#3-accessing-ref-values)
    - [4. **Reactive Objects with `ref`**](#4-reactive-objects-with-ref)
    - [Summary](#summary)
  - [ref in Vue compared to useRef and useState in React](#ref-in-vue-compared-to-useref-and-usestate-in-react)
    - [1. **React’s `useRef` vs. Vue’s `ref` for DOM Elements**](#1-reacts-useref-vs-vues-ref-for-dom-elements)
    - [2. **React’s `useState` vs. Vue’s `ref` for Reactive Data**](#2-reacts-usestate-vs-vues-ref-for-reactive-data)
    - [3. **Combining `useRef` and `useState` for More Complex Logic**](#3-combining-useref-and-usestate-for-more-complex-logic)
    - [**Summary**](#summary-1)


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

### Defining emitted events

By using defineEmits, you explicitly declare which events your component is capable of emitting. This improves the readability of your component by documenting what events are expected to be emitted, and it can also be used for validation.

```js
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
defineEmits(['enlarge-text'])
</script>
```

The tutorial also mentions "Typing Component Emits," which suggests that when using TypeScript, you can further enhance your event declarations by providing type definitions. This ensures type safety when emitting events and handling them in parent components.

## Content Distribution with Slots 

With slots you can pass any kind of content into a slot, including text, HTML, or even other Vue components.

In the parent component:

```js
<template>
  <AlertBox>
    Something bad happened.
  </AlertBox>
</template>
```

In the child component:
```js
<!-- AlertBox.vue -->
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot />
  </div>
</template>

<style scoped>
.alert-box {
  /* ... */
}
</style>
```

## Dynamic components

Sometimes, it's useful to dynamically switch between components, like in a tabbed interface. 
- Vue provides a special <component> element that dynamically switches between different components based on the value passed to its is attribute.
- The :is attribute is a binding (note the colon :) that determines which component to render based on the value it holds.

```js
<!-- Component changes when currentTab changes -->
<component :is="tabs[currentTab]"></component>
```

**Switching Components Dynamically**

The :is="tabs[currentTab]" binding makes Vue switch the rendered component whenever currentTab changes. This binding can either be the name of a registered component or an imported component object. Here, it’s an object, with each key pointing to the actual component (imported as Home, Posts, and Archive).

**Managing State with Refs**
   - `currentTab` is a reactive reference (`ref`) that holds the current tab’s name. This ensures that Vue re-renders the correct component whenever `currentTab` changes.
   - The buttons for each tab are generated using `v-for`, looping through the `tabs` object. When a button is clicked, `currentTab` is updated to the corresponding tab, triggering the dynamic component to change.

**Switching Components Dynamically**
   - The `:is="tabs[currentTab]"` binding makes Vue switch the rendered component whenever `currentTab` changes. This binding can either be the name of a registered component or an imported component object. Here, it’s an object, with each key pointing to the actual component (imported as `Home`, `Posts`, and `Archive`).

**Component Lifecycle**
   - By default, when a different component is rendered, the previous one is unmounted (removed from the DOM). This means that if you switch away from one tab, its component is destroyed. 
   - If you want the component to remain in memory (so it doesn’t lose its state), you can wrap the `<component>` tag in `<KeepAlive>`. This keeps the inactive components alive in memory.

**The Example Code Breakdown**
```js
<script setup>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
import { ref } from 'vue'

const currentTab = ref('Home')  // Reactive reference to keep track of the current tab

const tabs = {
  Home,    // Corresponds to the Home.vue component
  Posts,   // Corresponds to the Posts.vue component
  Archive  // Corresponds to the Archive.vue component
}
</script>

```

- This script imports the child components and sets up a reactive `currentTab` reference to track which tab is currently active. It also creates a `tabs` object that maps tab names to their respective components.

**Creating Tab Buttons and Rendering Dynamic Components**
```vue
<template>
  <div class="demo">
    <button
       v-for="(_, tab) in tabs"
       :key="tab"
       :class="['tab-button', { active: currentTab === tab }]"
       @click="currentTab = tab"
     >
      {{ tab }}
    </button>
    <component :is="tabs[currentTab]" class="tab"></component>
  </div>
</template>
```
- **Tab Buttons**: The buttons are generated using `v-for`, iterating over the `tabs` object. Each button displays the tab name and updates `currentTab` when clicked.
- **Dynamic Component Rendering**: The `<component>` element uses the `:is="tabs[currentTab]"` binding to display the correct component based on the current tab. When a button is clicked, `currentTab` changes, and the corresponding component is rendered inside the `<component>` tag.


## ref in Vue

In Vue.js, `ref` is a core concept used to create a reactive reference to a value or a DOM element. It is particularly useful when you need to track and react to changes in a piece of data, or when you need to directly interact with DOM elements in a component.

### 1. **Using `ref` for Reactive Data**

When you use `ref` to create a reactive reference to a value, Vue automatically tracks changes to that value and updates the DOM or any computed properties or watchers that depend on it. This is essential for reactivity in Vue components.

**Example:**
```vue
<script setup>
import { ref } from 'vue'

// Create a reactive reference to a number
const count = ref(0)

// Function to increment the count
function increment() {
  count.value++
}
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>
```

**Explanation:**
- `ref(0)` creates a reactive reference to the initial value `0`.
- `count.value` accesses the current value of the `count` reference.
- When `count.value` is incremented, the change is automatically reflected in the DOM.

### 2. **Using `ref` for DOM Elements**

In addition to creating reactive data, `ref` can also be used to directly reference DOM elements in your template. This is useful when you need to manipulate or access a DOM element programmatically.

**Example:**
```vue
<script setup>
import { ref } from 'vue'

// Create a ref to hold the DOM element
const inputRef = ref(null)

// Function to focus the input element
function focusInput() {
  inputRef.value.focus()
}
</script>

<template>
  <input ref="inputRef" type="text" placeholder="Focus me!" />
  <button @click="focusInput">Focus the input</button>
</template>
```

**Explanation:**
- `inputRef = ref(null)` creates a reference that will be linked to the input element.
- In the template, `ref="inputRef"` binds the DOM element to the `inputRef` reference.
- `inputRef.value` is the actual DOM element, so calling `inputRef.value.focus()` will focus the input field when the button is clicked.

### 3. **Accessing `ref` Values**

When using `ref`, the actual value is accessed through the `.value` property. This is important to remember, as it differentiates `ref` from Vue’s `reactive` function (which returns a reactive proxy).

### 4. **Reactive Objects with `ref`**

When using `ref` with an object or array, the object itself becomes reactive, but you still access it through the `.value` property.

**Example:**
```vue
<script setup>
import { ref } from 'vue'

const person = ref({
  name: 'Alice',
  age: 25
})

function updateName() {
  person.value.name = 'Bob'
}
</script>

<template>
  <p>Name: {{ person.value.name }}</p>
  <button @click="updateName">Change Name</button>
</template>
```

**Explanation:**
- `person` is a reactive reference to an object.
- Modifying `person.value.name` will trigger a re-render of any part of the template that depends on `person.value.name`.

### Summary

In Vue.js, `ref` is a versatile tool for creating reactive references to primitive values, objects, arrays, and DOM elements. It allows Vue to track changes to these values and update the DOM accordingly, making it a fundamental part of Vue's reactivity system. When using `ref`, remember to access the underlying value via the `.value` property.


## ref in Vue compared to useRef and useState in React

The `ref` feature in Vue is most similar to the `useRef` hook and the `useState` hook in React. These hooks allow you to work with both mutable values and direct references to DOM elements in a way that persists across re-renders.

### 1. **React’s `useRef` vs. Vue’s `ref` for DOM Elements**

In both React and Vue, `ref`/`useRef` is used to directly access and manipulate DOM elements.

**Example: Focusing an Input Element**
This example demonstrates how to use `ref` in Vue and `useRef` in React to focus an input element when a button is clicked.

**Vue.js Example:**

```vue
<script setup>
import { ref } from 'vue'

// Create a ref to hold the DOM element
const inputRef = ref(null)

// Function to focus the input element
function focusInput() {
  inputRef.value.focus()
}
</script>

<template>
  <input ref="inputRef" type="text" placeholder="Focus me!" />
  <button @click="focusInput">Focus the input</button>
</template>
```

**React Example:**

```jsx
import React, { useRef } from 'react';

function App() {
  // Create a ref to hold the DOM element
  const inputRef = useRef(null);

  // Function to focus the input element
  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Focus me!" />
      <button onClick={focusInput}>Focus the input</button>
    </div>
  );
}

export default App;
```

### 2. **React’s `useState` vs. Vue’s `ref` for Reactive Data**

In both React and Vue, `useState`/`ref` is used to create reactive data, which updates the UI when the data changes.

**Example: Counter**
This example shows how you can implement a simple counter using reactive data with `ref` in Vue and `useState` in React.

**Vue.js Example:**

```vue
<script setup>
import { ref } from 'vue'

// Create a reactive reference to a number
const count = ref(0)

// Function to increment the count
function increment() {
  count.value++
}
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>
```

**React Example:**

```jsx
import React, { useState } from 'react';

function App() {
  // Create a state variable and function to update it
  const [count, setCount] = useState(0);

  // Function to increment the count
  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default App;
```

### 3. **Combining `useRef` and `useState` for More Complex Logic**

For more complex use cases, you might combine `useRef` and `useState` in React, while in Vue, you'd typically use `ref` for both data and DOM references. Here’s an example where we combine them:

**Example: Controlled Input with Auto-Focus**
This example shows how to control an input's value reactively and automatically focus it when a button is clicked.

**Vue.js Example:**

```vue
<script setup>
import { ref } from 'vue'

// Reactive data for the input value
const inputValue = ref('')

// Create a ref to the input element
const inputRef = ref(null)

// Function to update the input value
function updateValue(event) {
  inputValue.value = event.target.value
}

// Function to focus the input element
function focusInput() {
  inputRef.value.focus()
}
</script>

<template>
  <div>
    <input 
      ref="inputRef" 
      type="text" 
      :value="inputValue" 
      @input="updateValue" 
      placeholder="Type something..."
    />
    <button @click="focusInput">Focus the input</button>
    <p>Input Value: {{ inputValue }}</p>
  </div>
</template>
```

**React Example:**

```jsx
import React, { useState, useRef } from 'react';

function App() {
  // State for the input value
  const [inputValue, setInputValue] = useState('');

  // Create a ref to the input element
  const inputRef = useRef(null);

  // Function to update the input value
  const updateValue = (event) => {
    setInputValue(event.target.value);
  };

  // Function to focus the input element
  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input 
        ref={inputRef} 
        type="text" 
        value={inputValue} 
        onChange={updateValue} 
        placeholder="Type something..."
      />
      <button onClick={focusInput}>Focus the input</button>
      <p>Input Value: {inputValue}</p>
    </div>
  );
}

export default App;
```

### **Summary**

- **React’s `useRef`** is similar to Vue’s `ref` when dealing with direct DOM references.
- **React’s `useState`** is analogous to Vue’s `ref` for managing reactive data.
- Vue’s `ref` is a more unified concept, handling both data reactivity and DOM element references, while in React, these responsibilities are split between `useRef` and `useState`.