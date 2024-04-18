# Tula

**Tu**ring **La**nguage. An Esoteric Programming Language based on [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine) extended with [Set Theory](https://en.wikipedia.org/wiki/Set_theory) and [S-expressions](https://en.wikipedia.org/wiki/S-expression).

## Base Syntax

The program consist of sequence of rules:

```js
case <State> <Read> <Write> <Step> <Next>
case <State> <Read> <Write> <Step> <Next>
case <State> <Read> <Write> <Step> <Next>
...
```

- `<State>` - The current state of the Machine,
- `<Read>` - What the Machine reads on the Tape,
- `<Write>` - What the Machine should write on the Tape,
- `<Step>` - Where the Head of the Machine must step (`<-` left or `->` right),
- `<Next>` - What is the next state of the Machine.

## Example

Simple program that increments a binary number (least significant bits come first):

```js
// Tape: 1 1 0 1

case Inc 0 1 -> Halt
case Inc 1 0 -> Inc
```

The trace of the execution of the above program:

```
Inc: 1 1 0 1
     ^
Inc: 0 1 0 1
       ^
Inc: 0 0 0 1
         ^
Halt: 0 0 1 1
            ^
```

## S-expressions

Symbols in the language could be also S-expressions. So you can have a Tape that consists of sequence of pairs of numbers:

``` js
// Tape: (1 2) (2 3) (3 4) &
```

## Sets and Universal Quantification

Tula supports defining Sets (which are collections of S-expression) and using [Universal Quantification](https://en.wikipedia.org/wiki/Universal_quantification) on those Sets to generate rules automatically.

A simple example that iterates the Tape of Pairs of Numbers and swaps each pair until it reaches the delimiter `&`:

```js
// Tape: (1 2) (2 3) (3 4) &

let Numbers { 1 2 3 4 }

for a in Numbers
for b in Numbers
case Swap (a b) (b a) -> Swap

case Swap & & -> Halt
```

The trace of the above program:

```
Swap: (1 2) (2 3) (3 4) &
      ^
Swap: (2 1) (2 3) (3 4) &
            ^
Swap: (2 1) (3 2) (3 4) &
                  ^
Swap: (2 1) (3 2) (4 3) &
                        ^
Halt: (2 1) (3 2) (4 3) & &
                          ^
```
