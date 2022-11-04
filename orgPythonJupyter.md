

## Create a Session

Initialize server with org-babel-exp-src-block.  Afterwards, 'Enter' in the sourceblock will execute.

```jupyter-python
print('ello, world!')
```


### Create a plot {#create-a-plot}

```jupyter-python
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])
pass
```


## Ocaml {#ocaml}

```ocaml
print_string "Hello world!\n"
```