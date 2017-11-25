# gulp-string-replace [![NPM version][npm-image]][npm-url] [![Build status][travis-image]][travis-url] [![dependencies][gulp-string-replace-dependencies-image]][gulp-string-replace-dependencies-url]
> Replaces strings in files by using string or regex patterns. Works with Gulp 3!

## Installation

### npm
```shell
npm install gulp-string-replace --save-dev
```

### yarn
```shell
yarn add --dev gulp-string-replace
```

## Usage

### Regex Replace
```javascript
var replace = require('gulp-string-replace');

gulp.task('replace_1', function() {
  gulp.src(["./config.js"]) // Any file globs are supported
    .pipe(replace(new RegExp('@env@', 'g'), 'production'))
    .pipe(gulp.dest('./build/config.js'))
});

gulp.task('replace_2', function() {
  gulp.src(["./index.html"])
    .pipe(replace(/version(={1})/g, '$1v0.2.2'))
    .pipe(gulp.dest('./build/index.html'))
});

gulp.task('replace_3', function() {
  gulp.src(["./config.js"])
    .pipe(replace(/foo/g, function () {
        return 'bar';
    }))
    .pipe(gulp.dest('./build/config.js'))
});
```
### String Replace
```javascript
gulp.task('replace_1', function() {
  gulp.src(["./config.js"])
    .pipe(replace('environment', 'production'))
    .pipe(gulp.dest('./build/config.js'))
});
```
### Function Replace
```javascript
gulp.task('replace_1', function() {
  gulp.src(["./config.js"])
    .pipe(replace('environment', function () {
        return 'production';
    }))
    .pipe(gulp.dest('./build/config.js'))
});

gulp.task('replace_2', function() {
  gulp.src(["./config.js"])
    .pipe(replace('environment', function (replacement) {
        return replacement + '_mocked';
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

gulp.task('replace_1', function() {
  gulp.src(["./config.js"])
    .pipe(replace(options)
    .pipe(gulp.dest('./build/config.js'))
});
```

### API

#### replace(options)

##### options
Type: `Object`

###### options.pattern
Type: `String` or `RegExp`

The string to search for.

###### options.replacement
Type: `String` or `Function`

The replacement string or function. Called once for each match.
Function has access to regex outcome (all arguments are passed).

###### options.logs.enabled
Type: `Boolean`, Default: `true`

Displaying logs.

###### options.logs.notReplaced
Type: `Boolean`, Default: `false`

Displaying "not replaced" logs.

More details here: [MDN documentation for RegExp] and [MDN documentation for String.replace].

#### replace(pattern, replacement, options)

##### pattern
Type: `String` or `RegExp`

The string to search for.

##### replacement
Type: `String` or `Function`

The replacement string or function. Called once for each match.
Function has access to regex outcome (all arguments are passed).

##### options
Type: `Object`

Same as above, but without properties `pattern` or `replacement`
