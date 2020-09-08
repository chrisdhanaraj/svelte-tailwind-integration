# Tailwind / Svelte Demo integration

Hi! This is a quick repo demonstration to how to quickly and painlessly integrate Tailwind into the Svelte pipeline, no additional scripts required! The main concept is that you'll need to leverage the postcss plugins that Tailwind uses directly, instead of relying on the Tailwind CLI. 

Then, use a `global` style block to import in Tailwind postcss plugins (check out src/App.svelte) to bring in Tailwind in!

# Steps

**Add Dependencies**

`yarn install -D tailwindcss autoprefixer svelte-preprocess @fullhuman/postcss-purgecss`

**Setup rollup config**

Check out the `rollup.config.js` for the full setup, but it's just plucked from the tailwindcss Getting Started walkthrough!

**1. Add the following blocks into your rollup.config**
```
const purgecss = require("@fullhuman/postcss-purgecss")({
  content: ["./src/**/*.svelte"],

  // This is the function used to extract class names from your templates
  defaultExtractor: (content) => {
    // Capture as liberally as possible, including things like `h-(screen-1.5)`
    const broadMatches = content.match(/[^<>"'`\s]*[^<>"'`\s:]/g) || [];

    // Capture classes within other delimiters like .block(class="w-1/2") in Pug
    const innerMatches = content.match(/[^<>"'`\s.()]*[^<>"'`\s.():]/g) || [];

    return broadMatches.concat(innerMatches);
  },
});

const preprocess = sveltePreprocess({
  sourceMap: !production,
  postcss: {
    plugins: [
      require("tailwindcss"),
      require("autoprefixer"),
      ...(process.env.NODE_ENV === "production" ? [purgecss] : []),
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

**3. Turn off the purge warnings for the build**

Tailwind will warn that it's not purging anything and if you're doing it manually to turn it off in config. Well, we're doing in manually so...

**3. Done!**