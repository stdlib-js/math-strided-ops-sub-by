<!--

@license Apache-2.0

Copyright (c) 2022 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

<!-- lint disable maximum-heading-length -->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# subBy

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Element-wise [subtraction][@stdlib/math/base/ops/sub] of two strided arrays via a callback function.

<section class="intro">

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import subBy from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-ops-sub-by@deno/mod.js';
```
The previous example will load the latest bundled code from the deno branch. Alternatively, you may load a specific version by loading the file from one of the [tagged bundles](https://github.com/stdlib-js/math-strided-ops-sub-by/tags). For example,

```javascript
import subBy from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-ops-sub-by@v0.2.1-deno/mod.js';
```

You can also import the following named exports from the package:

```javascript
import { ndarray } from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-ops-sub-by@deno/mod.js';
```

#### subBy( N, x, strideX, y, strideY, z, strideZ, clbk\[, thisArg] )

Performs element-wise [subtraction][@stdlib/math/base/ops/sub] of two strided arrays via a callback function and assigns each result to an element in an output strided array.

```javascript
function accessor( values ) {
    return values;
}

var x = [ 11.0, 12.0, 13.0, 14.0, 15.0 ];
var y = [ 8.0, 7.0, 6.0, 5.0, 4.0 ];
var z = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

subBy( x.length, x, 1, y, 1, z, 1, accessor );
// z => [ 3.0, 5.0, 7.0, 9.0, 11.0 ]
```

The function accepts the following arguments:

-   **N**: number of indexed elements.
-   **x**: input [`Array`][mdn-array], [`typed array`][mdn-typed-array], or an array-like object (excluding strings and functions).
-   **strideX**: index increment for `x`.
-   **y**: input [`Array`][mdn-array], [`typed array`][mdn-typed-array], or an array-like object (excluding strings and functions).
-   **strideY**: index increment for `y`.
-   **z**: output [`Array`][mdn-array], [`typed array`][mdn-typed-array], or an array-like object (excluding strings and functions).
-   **strideZ**: index increment for `z`.
-   **clbk**: callback function.
-   **thisArg**: execution context (_optional_).

The invoked callback function is provided four arguments:

-   **values**: input array element values `[vx, vy]`.
-   **idx**: iteration index (zero-based).
-   **indices**: input and output array strided indices `[ix, iy, iz]` (computed according to `offset + idx*stride`).
-   **arrays**: input and output arrays/collections `[x, y, z]`.

To set the callback execution context, provide a `thisArg`.

```javascript
function accessor( values ) {
    this.count += 1;
    return values;
}

var context = {
    'count': 0
};

var x = [ 11.0, 12.0, 13.0, 14.0, 15.0 ];
var y = [ 8.0, 7.0, 6.0, 5.0, 4.0 ];
var z = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

subBy( x.length, x, 1, y, 1, z, 1, accessor, context );
// z => [ 3.0, 5.0, 7.0, 9.0, 11.0 ]

var cnt = context.count;
// returns 5
```

The `N` and `stride` parameters determine which elements in the strided arrays are accessed at runtime. For example, to index every other value in `x` and to index the first `N` elements of `y` in reverse order,

```javascript
function accessor( values ) {
    return values;
}

var x = [ 11.0, 12.0, 13.0, 14.0, 15.0, 16.0 ];
var y = [ 8.0, 7.0, 6.0, 5.0, 4.0, 3.0 ];
var z = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

subBy( 3, x, 2, y, -1, z, 1, accessor );
// z => [ 5.0, 6.0, 7.0, 0.0, 0.0, 0.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

function accessor( values ) {
    return values;
}

// Initial arrays...
var x0 = new Float64Array( [ 11.0, 12.0, 13.0, 14.0, 15.0, 16.0 ] );
var y0 = new Float64Array( [ 8.0, 7.0, 6.0, 5.0, 4.0, 3.0 ] );
var z0 = new Float64Array( [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*3 ); // start at 4th element
var z1 = new Float64Array( z0.buffer, z0.BYTES_PER_ELEMENT*3 ); // start at 4th element

subBy( 3, x1, -2, y1, 1, z1, 1, accessor );
// z0 => <Float64Array>[ 0.0, 0.0, 0.0, 11.0, 10.0, 9.0 ]
```

#### subBy.ndarray( N, x, strideX, offsetX, y, strideY, offsetY, z, strideZ, offsetZ, clbk\[, thisArg] )

Performs element-wise [subtraction][@stdlib/math/base/ops/sub] of two strided arrays via a callback function and assigns each result to an element in an output strided array using alternative indexing semantics.

```javascript
function accessor( values ) {
    return values;
}

var x = [ 11.0, 12.0, 13.0, 14.0, 15.0 ];
var y = [ 8.0, 7.0, 6.0, 5.0, 4.0 ];
var z = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

subBy.ndarray( x.length, x, 1, 0, y, 1, 0, z, 1, 0, accessor );
// z => [ 3.0, 5.0, 7.0, 9.0, 11.0 ]
```

The function accepts the following additional arguments:

-   **offsetX**: starting index for `x`.
-   **offsetY**: starting index for `y`.
-   **offsetZ**: starting index for `z`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the offset parameters support indexing semantics based on starting indices. For example, to index every other value in `x` starting from the second value and to index the last `N` elements in `y`,

```javascript
function accessor( values ) {
    return values;
}

var x = [ 11.0, 12.0, 13.0, 14.0, 15.0, 16.0 ];
var y = [ 8.0, 7.0, 6.0, 5.0, 4.0, 3.0 ];
var z = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

subBy.ndarray( 3, x, 2, 1, y, -1, y.length-1, z, 1, 2, accessor );
// z => [ 0.0, 0.0, 0.0, 11.0, 10.0, 9.0 ]
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If a provided callback function does not return any value (or equivalently, explicitly returns `undefined`), the value is **ignored**.

    ```javascript
    function accessor() {
        // No-op...
    }

    var x = [ 11.0, 12.0, 13.0, 14.0, 15.0 ];
    var y = [ 8.0, 7.0, 6.0, 5.0, 4.0 ];
    var z = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

    subBy( x.length, x, 1, y, 1, z, 1, accessor );
    // z => [ 0.0, 0.0, 0.0, 0.0, 0.0 ]
    ```

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var uniform = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-uniform' ).factory;
import filledarray from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-filled@deno/mod.js';
import filledarrayBy from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-filled-by@deno/mod.js';
import subBy from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-ops-sub-by@deno/mod.js';

function accessor( values, i ) {
    if ( (i%3) === 0 ) {
        // Simulate a "missing" value...
        return;
    }
    return values;
}

var x = filledarrayBy( 10, 'generic', uniform( -10.0, 10.0 ) );
console.log( x );

var y = filledarrayBy( x.length, 'generic', uniform( -10.0, 10.0 ) );
console.log( y );

var z = filledarray( null, x.length, 'generic' );
console.log( z );

subBy.ndarray( x.length, x, 1, 0, y, -1, y.length-1, z, 1, 0, accessor );
console.log( z );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/math-strided-ops-sub-by.svg
[npm-url]: https://npmjs.org/package/@stdlib/math-strided-ops-sub-by

[test-image]: https://github.com/stdlib-js/math-strided-ops-sub-by/actions/workflows/test.yml/badge.svg?branch=v0.2.1
[test-url]: https://github.com/stdlib-js/math-strided-ops-sub-by/actions/workflows/test.yml?query=branch:v0.2.1

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/math-strided-ops-sub-by/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/math-strided-ops-sub-by?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/math-strided-ops-sub-by.svg
[dependencies-url]: https://david-dm.org/stdlib-js/math-strided-ops-sub-by/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/math-strided-ops-sub-by/tree/deno
[deno-readme]: https://github.com/stdlib-js/math-strided-ops-sub-by/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/math-strided-ops-sub-by/tree/umd
[umd-readme]: https://github.com/stdlib-js/math-strided-ops-sub-by/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/math-strided-ops-sub-by/tree/esm
[esm-readme]: https://github.com/stdlib-js/math-strided-ops-sub-by/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/math-strided-ops-sub-by/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/math-strided-ops-sub-by/main/LICENSE

[mdn-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/math/base/ops/sub]: https://github.com/stdlib-js/math-base-ops-sub/tree/deno

</section>

<!-- /.links -->
