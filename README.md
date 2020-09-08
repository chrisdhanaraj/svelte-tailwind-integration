# Tailwind / Svelte Demo integration

Hi! This is a quick repo demonstration to how to quickly and painlessly integrate Tailwind into the Svelte pipeline, no additional scripts required! The main concept is that you'll need to leverage the postcss plugins that Tailwind uses directly, instead of relying on the Tailwind CLI. 

Then, use a `global` style block to import in Tailwind postcss plugins (check out src/App.svelte) to bring in Tailwind in!

# Steps

**Add Dependencies**

`yarn install -D tailwindcss autoprefixer svelte-preprocess`

**Setup rollup config**

Check out the `rollup.config.js` for the full setup, but it's just plucked from the tailwindcss Getting Started walkthrough!

**1. Add the following blocks into your rollup.config**
```
const preprocess = sveltePreprocess({
  sourceMap: !production,
  postcss: {
    plugins: [
      require("tailwindcss"),
      require("autoprefixer"),
    ],
  },
});
```

**2. Add `preprocess` into the `svelte` rollup plugin**


```
plugins: [
  svelte({
    ...
    preprocess
  })
]
```

**3. Add a \<style global> block that references Tailwind**
```css
<style global>
  /* purgecss start ignore */
  @tailwind base;
  @tailwind components;
  /* purgecss end ignore */
  @tailwind utilities;
</style>
```

**4. Done!**