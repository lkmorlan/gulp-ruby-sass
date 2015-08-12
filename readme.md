# gulp-ruby-sass [![Build Status](https://travis-ci.org/sindresorhus/gulp-ruby-sass.svg?branch=master)](https://travis-ci.org/sindresorhus/gulp-ruby-sass)

Compiles Sass with [the Sass gem](http://sass-lang.com/install).  
To compile Sass with [libsass](http://libsass.org/), use [gulp-sass](https://github.com/dlmanning/gulp-sass)


## Install

You must have [Sass >=3.4](http://sass-lang.com/install).

```
$ npm install --save-dev gulp-ruby-sass
```


## Important!

- gulp-ruby-sass is a gulp source adapter. Use it instead of `gulp.src`.
- Since it's a source adapter you need to catch errors on the stream itself instead of using a package like plumber. See the Usage section for examples.
- gulp-ruby-sass doesn't support globs yet, only single files or directories. Just like Sass.
- gulp-ruby-sass doesn't support incremental builds yet ([issue](https://github.com/sindresorhus/gulp-ruby-sass/issues/111)).
- gulp-ruby-sass doesn't alter Sass's output in any way. Problems with Sass output should be reported to the [Sass issue tracker](https://github.com/sass/sass/issues).


## Usage

Use gulp-ruby-sass instead of `gulp.src` to compile a file or directory.  

```js
var gulp = require('gulp');
var sass = require('gulp-ruby-sass');

gulp.task('sass', function () {
	return sass('source/')
		.on('error', function (err) {
			console.error('Error!', err.message);
		})
		.pipe(gulp.dest('result'));
});
```

#### Recompiling on changes

Use [gulp-watch](https://github.com/gulpjs/gulp/blob/master/docs/API.md#gulpwatchglob--opts-tasks-or-gulpwatchglob--opts-cb) to automatically recompile your files on change.

#### Handling errors

Handle Sass errors with an `on('error', cb)` listener. gulp-ruby-sass throws errors like a gulp plugin, but streams the erroring files so you can see Sass errors in your browser too.

### Plugin options

#### verbose

Type: `boolean`  
Default: `false`

Gives some extra information for debugging, including the actual spawned Sass command.

#### bundleExec

Type: `boolean`  
Default: `false`

Run `sass` with [bundle exec](http://gembundler.com/man/bundle-exec.1.html).

#### sourcemap

Type: `boolean`  
Default: `false`

Requires Sass `>=3.4` and [`gulp-sourcemaps`](https://github.com/floridoo/gulp-sourcemaps).

*Inline sourcemaps* are recommended, as they "just work".

```js
var gulp = require('gulp');
var sass = require('gulp-ruby-sass');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('sass', function () {
	return sass('source', {sourcemap: true})
		.on('error', function (err) {
			console.error('Error!', err.message);
		})
		.pipe(sourcemaps.write())
		.pipe(gulp.dest('result'));
});
```

*File sourcemaps* require you to serve the sourcemap location so the browser can read the files. See the [`gulp-sourcemaps` readme](https://github.com/floridoo/gulp-sourcemaps) for more info.

```js
gulp.task('sass', function() {
	return sass('source', { sourcemap: true })
	.on('error', function (err) {
	  console.error('Error', err.message);
   })

	.pipe(sourcemaps.write('maps', {
		includeContent: false,
		sourceRoot: '/source'
	}))

	.pipe(gulp.dest('result'));
});
```

#### tempDir

Type: `String`  
Default: the OS temp directory as reported by [os-tempDir](https://github.com/sindresorhus/os-tmpdir)

This plugin compiles Sass files to a temporary directory before pushing them through the stream. Use `tempDir` to choose an alternate directory if you aren't able to use the default OS temporary directory.

#### container

Type: `String`  
Default: `gulp-ruby-sass`

If you only have a single gulp-ruby-sass task you can ignore this option. If you're running multiple gulp-ruby-sass tasks at once you must specify a separate `container` for each task to avoid file collisions. The value is appended to the `tempDir` option.

```js
var gulp = require('gulp');
var sass = require('gulp-ruby-sass');

gulp.task('sass-app', function () {
	return sass('source/app.scss', {container: 'app'})
		.on('error', function (err) {
			console.error('Error!', err.message);
		})
		.pipe(gulp.dest('result/app'));
});

gulp.task('sass-site', function () {
	return sass('source/site.scss', {container: 'site'})
		.on('error', function (err) {
			console.error('Error!', err.message);
		})
		.pipe(gulp.dest('result/site'));
});

gulp.task('sass', ['sass-app', 'sass-site']);
```

### Sass options

Any other options are passed directly to the Sass executable. The options are camelCase versions of Sass's dashed-case options.

The docs below list common options for convenience. Run `sass -h` for the complete list.

#### loadPath

Type: `string`, `array`  
Default: `false`

Import paths.

#### require

Type: `string`  
Default: `false`

Require a Ruby library before running Sass.

#### compass

Type: `boolean`  
Default: `false`

Make Compass imports available and load project configuration.

#### style

Type: `string`  
Default: `nested`

Output style. Can be nested (default), compact, compressed, or expanded.

#### force

Type: `boolean`  
Default: `false`

Recompile every Sass file, even if the CSS file is newer.

#### stopOnError

Type: `boolean`  
Default: `false`

If a file fails to compile, exit immediately.

#### defaultEncoding

Type: `string`  
Default: `false`

Specify the default encoding for input files.

#### unixNewlines

Type: `boolean`  
Default: `false`

Use Unix-style newlines in written files on non-Unix systems. Always true on Unix.

#### debugInfo

Type: `boolean`  
Default: `false`

Emit output that can be used by the FireSass Firebug plugin.

#### lineNumbers

Type: `boolean`  
Default: `false`

Emit comments in the generated CSS indicating the corresponding source line.

#### check

Type: `boolean`  
Default: `false`

Just check syntax, don't evaluate.

#### precision 

Type: `number`  
Default: `5`

How many digits of precision to use when outputting decimal numbers.

#### cacheLocation

Type: `string`  
Default: `false`

The path to save parsed Sass files. Defaults to .sass-cache.

#### noCache

Type: `boolean`  
Default: `false`

Don't cache parsed Sass files.

#### trace

Type: `boolean`  
Default: `false`

Show a full Ruby stack trace on error.

#### quiet

Type: `boolean`  
Default: `false`

Silence warnings and status messages during compilation.


## License

MIT © [Sindre Sorhus](http://sindresorhus.com)
