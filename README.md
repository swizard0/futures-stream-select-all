# futures-stream-select-all

A [futures](https://docs.rs/futures) library adapter for merging the output of several streams.

It implements a binary-tree structure to efficiently perform [select](https://docs.rs/futures/0.1.14/futures/stream/trait.Stream.html#method.select) for any amount of undelying streams.

## Usage

First, add this to your `Cargo.toml`:

```toml
[dependencies]
futures-stream-select-all = "0.1"
```

Next, add this to your crate:

```rust
extern crate futures-stream-select-all;

use futures_stream_select_all::select_all;
```

## Example

```rust
    use futures::{stream, Stream, Future};
    use futures_stream_select_all::select_all;

    let stream_a = stream::iter(vec![Ok(0), Ok(1)]);
    let stream_b = stream::iter(vec![Ok(2), Ok(3), Ok(4)]);
    let stream_c = stream::iter(vec![Ok(5)]);

    let mut values = select_all::<_, _, ()>(vec![stream_a, stream_b, stream_c])
        .collect()
        .wait()
        .unwrap();
    values.sort();
    assert_eq!(values, vec![0, 1, 2, 3, 4, 5]);
```

# License

`futures-stream-select-all` is primarily distributed under the terms of both the MIT license and
the Apache License (Version 2.0), with portions covered by various BSD-like
licenses.

See LICENSE-APACHE, and LICENSE-MIT for details.
