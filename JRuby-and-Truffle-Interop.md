JRuby supports standard Truffle interop messages. This document explains what it does when it receives them, how to get it to explicitly send them, how to get it to send them using more idiomatic Ruby, and how what messages it sends for normal Ruby operations on foreign objects.

## How JRuby responds to messages

### `IS_EXECUTABLE`

Todo

### `EXECUTE`

Todo

### `INVOKE`

Todo

### `HAS_SIZE`

Returns true only for instances of `Array`, `Hash` and `String`.

### `GET_SIZE`

Call `size` on the object.

### `IS_BOXED`

Returns true only for instances of `String` with a length of 1, which allows them to be unboxed as a character.

### `UNBOX`

Todo

### `IS_NULL`

Todo

### `READ`

Todo

### `WRITE`

Todo

## How to explicitly send messages from JRuby

### `IS_EXECUTABLE`

Todo

### `EXECUTE`

Todo

### `INVOKE`

Todo

### `HAS_SIZE`

Todo

### `GET_SIZE`

Todo

### `IS_BOXED`

Todo

### `UNBOX`

Todo

### `IS_NULL`

Todo

### `READ`

Todo

### `WRITE`

Todo

## How to send messages using idiomatic Ruby

### `IS_EXECUTABLE`

Todo

### `EXECUTE`

Todo

### `INVOKE`

Todo

### `HAS_SIZE`

Todo

### `GET_SIZE`

Todo

### `IS_BOXED`

Todo

### `UNBOX`

Todo

### `IS_NULL`

Todo

### `READ`

Todo

### `WRITE`

Todo

## What messages are sent for Ruby syntax on foreign objects

Todo
