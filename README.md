# Experiments with Fused Braids

This repository contains a few experiments with "fused-crossing braids" -- braids where we add a few more generators to represent non-modifiable topology black boxes ("fused crossings"), with the goal of making machine-knit topology representable.

## Conventions

For these hacks, we will think of generators as being functions that are composed by multiplication. So the rightmost generator will be applied "first" and the leftmost will be applied "last".
When drawing braid words, we will draw the braid with time running bottom-to-top, so the leftmost generator will be at the top of the picture and the rightmost generator will be at the bottom.

## Generators

### Basic Braid Generators

Our system uses basic strand-crossing generators as in the standard Artin Braid Group presentation:

`si` crosses strand i over strand i+1:
```
  |   |
   \ /
    /
   / \
  |   |
  i  i+1
```

`Si` crosses strand i under stand i:
```
  |   |
   \ /
    \
   / \
  |   |
  i  i+1
```

For reasons that will become more evident below, the "formal inverse" (`~`) of the strand-crossing generators are just their front-back flips. (That is: `si~` == `Si` and `Si~` == `si`.)

### Fused Generators

Fused generators have "front" and "back" versions, drawn with a filled or empty dot:

`xi` glues strand i over strand i+1:
```
  |   |
   \ /
    ●
   / \
  |   |
  i  i+1
```


`Xi` glues strand i under strand i+1:
```
  |   |
   \ /
    ○
   / \
  |   |
  i  i+1
```

`yi` splits strand i into two strands ('front' version):
```
 i-1 i  i+1 i+2
  |  |   |   |
  |   \ /    |
  |    ●     |
  |    |     |
 i-1   i    i+1
```


`Yi` splits strand i into two strands ('back' version):
```
 i-1 i  i+1 i+2
  |  |   |   |
  |   \ /    |
  |    ○     |
  |    |     |
 i-1   i    i+1
```

`ni` joins strand i and i + 1 ('front' version):
```
 i-1   i    i+1
  |    |     |
  |    ●     |
  |   / \    |
  |  |   |   |
 i-1 i  i+1 i+2
```

`Ni` joins strand i and i + 1 ('back' version):
```
 i-1   i    i+1
  |    |     |
  |    ○     |
  |   / \    |
  |  |   |   |
 i-1 i  i+1 i+2
```

These also all have formal inverses, denoted by appending `~`, which are drawn with filled/empty boxes:

`xi~`:
```
  |   |
   \ /
    ■
   / \
  |   |
  i  i+1
```

`Xi~`:
```
  |   |
   \ /
    □
   / \
  |   |
  i  i+1
```

The formal inverses of `y` and `n` are flipped vertically, as you'd expect:

`yi~`:
```
 i-1   i    i+1
  |    |     |
  |    ■     |
  |   / \    |
  |  |   |   |
 i-1 i  i+1 i+2
```

`ni~`:
```
 i-1 i  i+1 i+2
  |  |   |   |
  |   \ /    |
  |    ■     |
  |    |     |
 i-1   i    i+1
```

(`Yi~`, `Ni~` are similar with empty squares.)



# OLD TEXT

## Identities

List may incorrect / incomplete.

### Basic Braid Identities

Crossings that don't interfere can slide past each-other:

```
σ_i σ_j = σ_j σ_i if |j-i| > 1
si sj = sj si if |j-i| > 1

  |   |  |   |     |   |  |   |
   \ /   |   |     |   |   \ /
    /    |   |     |   |    /
   / \   |   |  =  |   |   / \
  |   |   \ /       \ /   |   |
  |   |    /         /    |   |
  |   |   / \       / \   |   |
  |   |  |   |     |   |  |   |
```

(this also works for inverses and fixes)

Crossings can slide past unrelated strands:

```
si si+1 si = si+1 si si+1

  |   |   |     |   |   |
   \ /    |     |    \ /
    /     |     |     / 
   / \    |  =  |    / \
  |   |   |     |   |   |
  |    \ /       \ /    |
  |     /         /     |
  |    / \       / \    |
  |   |   |     |   |   |
   \ /    |     |    \ /
    /     |     |     /
   / \    |     |    / \
  |   |   |     |   |   |
```

(also works for inverses and fixes)

Of special note are uneven fixes, which require renumbering to happen on the basis elements:

```
s1 y3 <-> y3 s1

  |   |  |   |     |   |  |   |
   \ /   |   |     |   |   \ /
    /    |   |     |   |    y
   / \   |   |  =  |   |    |
  |   |   \ /       \ /     |
  |   |    y         /      |
  |   |    |        / \     |
  |   |    |       |   |    |
  1   2    3       1   2    3
```


```
y1 s2 <-> s3 y1

  |   |  |   |     |   |  |   |
   \ /   |   |     |   |   \ /
    y    |   |     |   |    /
    |    |   |  =  |   |   / \
    |     \ /      |   |  |   |
    |      /        \ /   |   |
    |     / \        y    |   |
    |    |   |       |    |   |
    1    2   3       1    2   3
```


```
y1 s1 <-> y2 s1 s2

  |   |   |     |   |   |
   \ /    |     |    \ /
    y     |     |     / 
     \    |  =  |    / \
      \   |     |   |   |
       \ /       \ /    |
        /         /     |
       / \       / \    |
      |   |     |   |   |
      |   |     |    \ /
      |   |     |     y
      |   |     |     |
      1   2     1     2
```

This might suggest that a (pointer-based?) representation of the generators would be more suitable.
(Note that I'm using `<->` because these are transformations but not equalities -- you can't do the substitution at will because other generators might require renumbering.)

## How does this connect to knitting?

Every live loop and yarn is a strand, and are held in left-to-right, back-to-front (perhaps?) order at the current racking.

Yarn movement and racking both generate `si` / `Si` (crossing) sequences.

`knit`ing and `tuck`ing generate `oi` / `xi` if loop and yarn are already in;
`yi` / `gi` if the yarn is not yet in or the needle is empty;
`vi` / `ui` if the yarn is not yet in *and* the needle is empty;
and `ji` / `ki` / `ni` / `zi` if the yarn is taken out or the loop is dropped after.

`xfer` instructions either don't do anything or generate `yi` / `gi` (depending on whether the destination needle is empty).

`split` instructions might need a custom unbalanced operation because they involve a two-in, three-out "knit and return loop" output. Could probably model them as a clump of `x`, `y` and so on.

Yarn in and out, along with loop drops could be implemented by having special `f01` / `f10` ops, and then chasing these back to the nearest fused op in a pre-process.
