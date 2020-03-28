# Create a Vue.js component library

## Prerequisites

We will use [yarn](https://yarnpkg.com/) as package manager and [vue-cli](https://cli.vuejs.org/) to create our library.

Check that you have already installed yarn by typing:

```shell
yarn --version
```

If you need to install it follow [this guide](https://yarnpkg.com/lang/en/docs/install/).

Let's now proceed with the installation of vue-cli:

```shell
yarn global add @vue/cli
```

---

## The library

#### Basic configurations

Let's start writing our library📦, don't worry initially it will be nothing more than a Vue.js application like the others that we will later convert to a library:

```shell
vue create mylib
```

You can replace `mylib` with any string to give the name you prefer to your library, the important thing and that you remember to replace this string also in all the code that will follow.

You will be asked the following:

```shell
Vue CLI v4.2.3
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint)
  Manually select features
```

Feel free to press enter without changing anything, so as to have a preset of configurations that will help us write cleaner code.

Now let's get ready to write some code, access the library folder using your favorite editor, if you don't have one, I recommend using [visual studio code](https://code.visualstudio.com/):

```shell
cd mylib
```

Open the `package.json` file, in the `scripts` section you will notice the following:

```json
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint"
},
```

This is the list of commands that, by default, are already available to us:

- `serve` start a development server to be used locally to develop our application;
- `build` it is designed to create a version ready for the production of the app;
- `lint` it is used to check whether the code complies with the guidelines;

As mentioned earlier, a library differs from an app in the way it is built. By default, the "_build in library_ mode" is not available. To add it, insert the `build-lib` command among the`scripts`:

```json
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "build-lib": "vue-cli-service build --target lib --name mylib src/main.js",
  "lint": "vue-cli-service lint"
},
```

Again you can replace `build-lib` with a string of your choice.

Ora provando ad eseguire il comando `yarn build-lib` dovresti vedere qualcosa di simile a questo:

```shell
 DONE  Compiled successfully in 1301ms                                                                                                                                                                     11:38:29

  File                        Size                    Gzipped

  dist/mylib.umd.min.js    12.75 KiB               4.86 KiB
  dist/mylib.umd.js        40.65 KiB               10.46 KiB
  dist/mylib.common.js     40.27 KiB               10.34 KiB

  Images and other types of assets omitted.

✨  Done in 5.78s.
```

This indicates that the compilation was successful. Looking inside the `dist` folder you should see several files.

We define which file is to be used by the apps that will import our package. We decide that it is `common.js`. To do this, add the following line in your `package.json` file:

```json
"main": "./dist/mylib.common.js"
```

---

#### Component creation

We proceed to the creation of the component that we will supply thanks to our library. Create a new file called `Picture.vue` inside the`components` folder.

The component in question will be somewhat basic, it will simply print the `img` tag and populate its main attributes (`src` and `alt`) or alternatively show a placeholder.

Here is the code of our component `Picture.vue`

```
<template>
  <div>
    <img :src="imageSrc" :alt="alt" />
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      default: ""
    },
    alt: {
      type: String,
      default: ""
    },
    placeholder: {
      type: String,
      default: "https://picsum.photos/600/400"
    }
  },
  computed: {
    imageSrc() {
      return this.src ? this.src : this.placeholder;
    }
  }
};
</script>
```

`vue-cli` also gives us the opportunity to see our component in action:

```
vue serve src/components/Picture.vue
```

Finally, in order to import our library into any Vue.js app, we need to define what will be exposed.

Open the `main.js` file, and replace the entire contents with the following:

```
import Picture from './components/Picture.vue'
export default Picture
```

The above code is very simple: we import our newly created component and expose it.

To keep your code cleaner you can remove the `src/components/HelloWorld.vue` and `src/App.vue` files which are no longer used.

Don't forget to update the library by running the `yarn build-lib` command.

**Great, our first library is ready!**🎉

## Importing the library into an app

Well now that the library is ready we both know that you can't wait to import it into a Vue.js app to be able to verify that everything is working properly, to do this we create a Vue.js app in which to use our new library.🔥

#### Creating an app

Remembering to leave the directory of our library `cd ..` and create a Vue.js app in which to import our new library:

```shell
vue create myapp
```

---

#### Library import

Usually you will be used to importing libraries directly obtained from the npm register, but for simplicity we take advantage of the possibility given by `yarn` to import a local library.

```shell
yarn add ../mylib
```

In doing so, you just added the `mylib` library as a dependency in the Vue.js app `myapp` and you can check this by opening the `myapp/package.json` inside `dependencies` you should see:

```json
"dependencies": {
  ...
  "mylib": "../mylib",
  ...
},
```

---

#### How to use the component

Great! You are ready to use the component provided by your library, to do this we clean up the `myapp/src/App.vue` file by replacing all its contents with:

```javascript
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <Picture v-bind="myPicture" />
  </div>
</template>

<script>
import Picture from 'mylib'
export default {
  name: 'App',
  components: {
    Picture,
  },
  data() {
    return {
      myPicture: {
        src: 'https://picsum.photos/600/400',
        alt: 'My Picture',
      },
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

We just have to check if the component of our library is showing the image, let's serve myapp locally to find out:

```shell
yarn serve
```

If all went well you should see the following:

![mylib component](https://nncb.netlify.com/images/uploads/mylib-picture-component.png "My Lib Component")

That's all for now. We have acquired the basics necessary to create a custom library by creating a simple component that can be easily used in any Vue.js app.🍻
