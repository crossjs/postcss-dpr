# postcss-flexible

This is a [postcss](https://www.npmjs.com/package/postcss) plugin for translating `dpr(...)`, `rem(...)`, `url(...)`. Similar to [px2rem](https://github.com/songsiqi/px2rem-postcss), but using custom function istead of comments for syntax.

[![Travis](https://img.shields.io/travis/crossjs/postcss-flexible.svg?style=flat-square)](https://travis-ci.org/crossjs/postcss-flexible)
[![Coveralls](https://img.shields.io/coveralls/crossjs/postcss-flexible.svg?style=flat-square)](https://coveralls.io/github/crossjs/postcss-flexible)
[![dependencies](https://david-dm.org/crossjs/postcss-flexible.svg?style=flat-square)](https://david-dm.org/crossjs/postcss-flexible)
[![devDependency Status](https://david-dm.org/crossjs/postcss-flexible/dev-status.svg?style=flat-square)](https://david-dm.org/crossjs/postcss-flexible?type=dev)
[![NPM version](https://img.shields.io/npm/v/postcss-flexible.svg?style=flat-square)](https://npmjs.org/package/postcss-flexible)

## Usage

### Webpack

```js
module.exports = {
  module: {
    loaders: [
      {
        test: /\.css$/,
        loader: "style-loader!css-loader!postcss-loader"
      }
    ]
  },
  postcss: function() {
    // remUnit defaults to 75
    return [require('postcss-flexible')({remUnit: 75})]
  }
}
```

## Example

Before processing:

```css
.selector {
  font-size: dpr(32px);
  width: rem(75px);
  line-height: 3;
  /* replace all @[1-3]x in urls: `/a/@2x/x.png`, `/a@2x/x.png` or `/a/x@2x.png` */
  background-image: url(/images/qr@2x.png);
}
```

After processing:

```css
.selector {
  width: 1rem;
  line-height: 3;
}

[data-dpr="1"] .selector {
  font-size: 16px;
  background-image: url(/images/qr@1x.png);
}

[data-dpr="2"] .selector {
  font-size: 32px;
  background-image: url(/images/qr@2x.png);
}

[data-dpr="3"] .selector {
  font-size: 48px;
  background-image: url(/images/qr@3x.png);
}
```

## options

- `desktop`: boolean, default `false`
- `baseDpr`: number, default `2`
- `remUnit`: number, default `75`
- `remPrecision`: number, default `6`
- `addPrefixToSelector`: function
- `dprList`: array, default `[3, 2, 1]`
- `fontGear`: array, default `[-1, 0, 1, 2, 3, 4]`
- `enableFontSetting`: boolean, default `false`
- `addFontSizeToSelector`: function
- `outputCSSFile`: function

## Change Log

* add: generate different css files with fontGear
* support custom `fontGear`
* support custom `enableFontSetting`
* support custom `addFontSizeToSelector`
* support custom `outputCSSFile`


### 0.5.0

* support custom `dprList`

### 0.4.0

* support custom `dprList`

### 0.3.0

* add option `desktop` and `addPrefixToSelector`

### 0.1.0

* handle `url()`

### 0.0.3

* add `dpr()` and `rem()`

### 0.0.0

* First release.

## License

MIT
