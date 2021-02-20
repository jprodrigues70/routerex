# Routerex

A vue-router extension to pre-configure routes, middlewares and layouts.

With routerex you save yourself the hassle of managing a route file in a vue application that uses vue-router. Your project folder organization will determinate the routes, but you can set it in page too.

## Example

If you want a `/user` route, you can do:

```
pages/user/index.vue
```

or in your `pages/something/index.vue`:

```html
<template> </template>
<script>
  export default {
    name: "UserPage",
    route: "/user",
  };
</script>
```

## With $route.params

You will put a `_` prefix in yout file name. Ex:

```
pages/user/_id.vue
```

or in your `pages/something/_id.vue`:

```html
<template>...</template>
<script>
  export default {
    name: "ShowUserPage",
    route: "/user/:id",
    mounted() {
      console.log(this.$route.params);
    },
  };
</script>
```

This will permit you access `/user/2` for example. You can put more levels with new params using folders. Ex: `pages/product/_id/_slug.vue`.

## Middlewares

You can set a array of middlewares that will execute before page enter. Ex:

```html
<template>...</template>
<script>
  export default {
    name: "Login",
    middleware: ["guest"],
    route: "/login",
  };
</script>
```

So in a folder named `middlewares` you will need a file called `guest`

```js
export default function ({ redirect, next }) {
  const user = localStorage.user ? JSON.parse(localStorage.user) : null;
  const token = localStorage.token ? localStorage.token : null;

  if (user && token) {
    redirect("/");
  }

  next();
}
```

Middlewares receives a context:

```js
ctx = {
  _component,
  to,
  from,
  redirect: next,
  next,
};
```

## Layouts

You can set a default layout with preset style or behavior

```html
<template>...</template>
<script>
  export default {
    name: "Login",
    middleware: ["guest"],
    route: "/login",
    layout: "default",
  };
</script>
```

So, in a folder called `layouts`, you will need a file called `default.vue`

```html
<template>
  <div class="l-default">
    <slot />
  </div>
</template>
<script>
  export default {
    name: "default",
    mounted() {
      console.log("default layout");
    },
  };
</script>
```
