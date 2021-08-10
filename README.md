# Wasm Game of Life

This project explores rust and webassembly following [Rust and WebAssembly Tutorial](https://rustwasm.github.io/docs/book/introduction.html)

## Prerequisites:
Tools:
- Rust
  - core: 
    - rustup, rustrc, cargo
  - ecosystem:
    - wasm-pack: convert rust to WebAssembly to JavaScript
    - cargo-generate (optional)
    - npm init template `create-wasm-app`
- npm & nodejs

## Development
- Workflow: 
  - Rust > WebAssembly > JavaScript
    1. build rust project into WebAssembly and JavaScript: `wasm-pack build`
    2. run the app from within app with normal npm commands
- Key Concepts in development (notes from tutorial):
  - interfacing Rust and JavaScript
    1. Minimizing copying into and out of the WebAssembly linear memory. 
      Unnecessary copies impose unnecessary overhead.
    2. Minimizing serializing and deserializing. 
      Similar to copies, serializing and deserializing also imposes overhead, and often imposes copying as well. If we can pass opaque handles to a data structure — instead of serializing it on one side, copying it into some known location in the WebAssembly linear memory, and deserializing on the other side — we can often reduce a lot of overhead. wasm_bindgen helps us define and work with opaque handles to JavaScript Objects or boxed Rust structures.
    
    **As a general rule of thumb**:
      a good JavaScript↔WebAssembly interface design is often one where large, long-lived data structures are implemented as Rust types that live in the WebAssembly linear memory, and are exposed to JavaScript as opaque handles. 
      
      JavaScript calls exported WebAssembly functions that take these opaque handles, transform their data, perform heavy computations, query the data, and ultimately return a small, copy-able result. 
      
      By only returning the small result of the computation, we avoid copying and/or serializing everything back and forth between the JavaScript garbage-collected heap and the WebAssembly linear memory.


## Credits
The project is generated using the following templates:
- [wasm-pack-template](https://github.com/rustwasm/wasm-pack-template)
- [create-wasm-app](https://github.com/rustwasm/create-wasm-app)

## 🔋 Batteries Included

* [`wasm-bindgen`](https://github.com/rustwasm/wasm-bindgen) for communicating
  between WebAssembly and JavaScript.
* [`console_error_panic_hook`](https://github.com/rustwasm/console_error_panic_hook)
  for logging panic messages to the developer console.
* [`wee_alloc`](https://github.com/rustwasm/wee_alloc), an allocator optimized
  for small code size.
