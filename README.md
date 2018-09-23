# gulp-replace-string
> Replaces strings in files by using string or regex patterns.

You'll actually want to instead use [@yodasws/gulp-pattern-replace](https://github.com/YodasWs/gulp-pattern-replace#readme) ([npm](https://www.npmjs.com/package/@yodasws/gulp-pattern-replace)). It provides extra flexability and smaller dependency tree.

Forked this off [gulp-string-replace](https://github.com/tomaszczechowski/gulp-string-replace) because I wanted to be able to pass all the arguments/options in a single [options object argument](https://github.com/YodasWs/gulp-replace-string#example-with-options-object).

## Installation

### yarn
```shell
yarn add --dev gulp-replace-string
```

### npm
```shell
npm install --save-dev gulp-replace-string
```

## Usage

### Regex Replace
```javascript
var replace = require('gulp-replace-string');

gulp.task('replace_1', () => {
  gulp.src(["./config.js"])
    .pipe(replace(new RegExp('@env@', 'g'), 'production'))
    .pipe(gulp.dest('./build/config.js'))
});

gulp.task('replace_2', () => {
  gulp.src(["./index.html"])
    .pipe(replace(/version(={1})/g, '$1v0.2.2'))
    .pipe(gulp.dest('./build/index.html'))
});

gulp.task('replace_3', () => {
  gulp.src(["./config.js"])
    .pipe(replace(/foo/g, () => 'bar'))
    .pipe(gulp.dest('./build/config.js'))
});
```

### String Replace
```javascript
gulp.task('replace_1', () => {
  gulp.src(["./config.js"])
    .pipe(replace('@env@', 'production'))
    .pipe(gulp.dest('./build/config.js'))
});
```

### Function Replace
```javascript
gulp.task('replace_1', () => {
  gulp.src(["./config.js"])
    .pipe(replace('@env@', () => {
        return argv.env === 'dev' ? 'dev' : 'production';
    }))
    .pipe(gulp.dest('./build/config.js'))
});

gulp.task('replace_2', () => {
  gulp.src(["./config.js"])
    .pipe(replace('environment', (pattern) => {
        return pattern + '_mocked';
    }))
    .pipe(gulp.dest('./build/config.js'))
});
```

### Example with options object
```javascript
var options = {
  pattern: /@env@/g
  replacement: 'dev',
  logs: {
    enabled: false
  }
};

gulp.task('replace_1', () => {
  gulp.src(["./config.js"])
    .pipe(replace(options)
    .pipe(gulp.dest('./build/config.js'))
});
```

### An Array
```javascript
gulp.task('lint-js', () => {
  gulp.src(["./config.js"])
    .pipe(replace([/(if|for|switch|while)\(/g, '$1 (')
    .pipe(gulp.dest('./build/config.js'));
});
```

## API

### replace(options)

#### options
Type: `Object`

##### options.pattern
Type: `String` or `RegExp`

The string to search for.

##### options.replacement
Type: `String` or `Function`

The replacement string or function. Called once for each match.
Function has access to regex outcome (all arguments are passed).

More details here: [MDN documentation for RegExp] and [MDN documentation for String.replace].

##### options.logs
Type: `Boolean` or `Object`

Output logs.

`true` is the same as:
```javascript
logs: {
  enabled: true,
  notReplaced: false,
}
```

##### options.logs.enabled
Type: `Boolean`, Default: `true`

Output logs.

##### options.logs.notReplaced
Type: `Boolean`, Default: `false`

Output "not replaced" logs.

### replace(pattern, replacement, options)

#### pattern
Type: `String` or `RegExp`

The string to search for.

#### replacement
Type: `String` or `Function`

The replacement string or function. Called once for each match.
Function has access to regex outcome (all arguments are passed).

#### options
Type: `Object`

Same as above, but without properties `pattern` or `replacement`
