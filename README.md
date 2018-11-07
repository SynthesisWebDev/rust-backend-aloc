# rust-backend-aloc

The goal: To rewrite the api currently managing data through redux into rust, interfacing with Holochain.

Here are the rust docs for the language itself:
https://doc.rust-lang.org/book/2018-edition/ch02-00-guessing-game-tutorial.html

Here is the guide on how to manage data to/from the DHT:

https://holochain.github.io/holochain-rust/

How to install holochain rust in your system:
https://github.com/holochain/app-spec-rust/releases

A working rust Api set up with holochain:

https://github.com/holochain/app-spec-rust

A working app example:
https://github.com/holochain/tasktaskic

A tutorial on how to go about this:
https://github.com/holochain/tasktaskic

The general approach:

- start with the tutorial, follow it and make it work, then do it again, but change out for one of the functions of the app, then repeat for the rest of the functions in a reducer, then for the rest of the reducers.

- Alternate between the above, and taking tutorials in rust.
