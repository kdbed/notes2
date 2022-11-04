+++
title = "Haskell - Org Test"
author = ["svejk"]
draft = false
+++

## Map {#map}

```haskell
import Control.Monad

:{

  map
      (\x -> x*x + x + 1)
      [1..10]
:}
```

```text
 [3,7,13,21,31,43,57,73,91,111]
```