# Bundler plugins

### Universal options

- `minifyProperties` - minify the property names of style objects. Unless you pass custom objects to `style9()` not generated by `style9.create()`, this is safe to enable and will lead to smaller JavaScript output but mangled property names. Consider enabling it in production. Default: `false`

## Webpack

```javascript
const Style9Plugin = require('style9/webpack');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  module: {
    rules: [
      {
        test: /\.(tsx|ts|js|mjs|jsx)$/,
        use: Style9Plugin.loader,
        options: {
          parserOptions?: BabelParserOpts;
          minifyProperties?: boolean;
        }
      },
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      }
    ]
  },
  plugins: [
    new Style9Plugin(),
    new MiniCssExtractPlugin()
  ]
};
```

## Rollup

```javascript
import style9 from 'style9/rollup';

export default {
  // ...
  plugins: [
    style9({
      include?: (string | RegExp)[] | string | RegExp;
      exclude?: (string | RegExp)[] | string | RegExp;
      // fileName XOR name is required
      fileName: string; // will be emitted as fileName
      name: string; // will be emitted according to output.assetFileNames format
      parserOptions?: BabelParserOpts;
      minifyProperties?: boolean;
    })
  ]
};
```

## Next.js

```javascript
const withTM = require('next-transpile-modules')(['style9']);
const withStyle9 = require('style9/next');

module.exports = withStyle9({
  parserOptions?: BabelParserOpts;
  minifyProperties?: boolean;
})(withTM());
```

## Gatsby

```javascript
module.exports = {
  plugins: [
    {
      resolve: 'style9/gatsby',
      options: {
        parserOptions?: BabelParserOpts;
        minifyProperties?: boolean;
      }
    }
  ]
}
```

## Babel

When using the babel plugin you'll have to pass the generated CSS to the post-processor yourself. If compiling multiple files this should be done to the concatenated output of all babel transforms.

```javascript
const babel = require('@babel/core');
const processCSS = require('style9/src/process-css');

const output = babel.transformFile('./file.js', {
  plugins: [['style9/babel', {
    minifyProperties?: boolean;
  }]]
});
const css = processCSS(output.metadata.style9 || '').css;
```