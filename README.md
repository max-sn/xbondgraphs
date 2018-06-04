The `xbondgraphs` package
===========
[xbondgraphs] is a LaTeX package that defines Ti*k*z styles and PGF keys to draw beautiful bond graphs.

# Table of contents
<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:1 -->

1. [Table of contents](#table-of-contents)
2. [Installing](#installing)
3. [Functionality](#functionality)
4. [Examples](#examples)
5. [Alternatives](#alternatives)
	1. [Functionality comparison](#functionality-comparison)

<!-- /TOC -->

# Installing
The package can be installed by running the `make sty` command from the command line. The generated `xbondgraphs*.sty` files must be copied to a location searched by TeX, under `$TEX_ROOT$/tex/latex/xbondgraphs`. Currently the files generated by `make sty` are:
* `xbondgraphs.sty`
* `xbondgraphs.iconix.sty`

To generate the accompanying manual, the `make doc` command can be run from the command line. The generated `xbondgraphs.pdf` must be copied to `$TEX_ROOT$/doc/latex/xbondgraphs`.

`$TEX_ROOT$` is the root folder of your main TeX-tree. For MiKTeX users that is probably `C:/texmf/`, and for TeXLive users that is probably `/usr/local/texlive/<year>/texmf-dist/`.

Alternatively one can use the `make all` command to execute both of the above.

# Functionality
With [xbondgraphs], users can:
* Use the `\bge` macro to draw in-line bond graph elements
* Use the `\bond` macro to draw in-line bonds
* Use the `bond` key for every path command (e.g. `\draw`, `\path`) to create bonds in a `tikzpicture` environment
* Use the `bond graph element` or the `bge` key in a `\node` specification to create elements in a `tikzpicture` environment
* Use the `mux` key in a `\node` specification to create a mux element in a `tikzpicture` environment with a specified number of in- and outputs.

# Examples
```latex
\begin{tikzpicture}[x=12.5mm,y=12.5mm]
    \node (u) at (-1, 0) [bge={Se}{},pin={left:$ u $}];
    \node (i) at ( 0, 0) [bge={1}{}, label={300:$ i $}];
    \node (r) at ( 0, 1) [bge={R}{}, pin={above:$ R $}];
    \node (l) at ( 0,-1) [bge={I}{}, pin={below:$ L $}];
    \node (k) at ( 1, 0) [bge={GY}{},pin={below:$ K_m $}];
    \node (w) at ( 2, 0) [bge={1}{}, label={300:$ \omega $}];
    \node (d) at ( 2, 1) [bge={R}{}, pin={above:$ D $}];
    \node (j) at ( 2,-1) [bge={I}{}, pin={below:$ J $}];
    \graph {
        (u) ->[bond={effort out}] (i) ->[bond={effort in}] (k)
            ->[bond={effort out}] (w) ->[bond={effort in}] (d);
        (i) ->[bond={effort out}] (l);
        (i) ->[bond={effort in}] (r);
        (w) ->[bond={effort out}] (j)};
\end{tikzpicture}
```
<center>
<img src="/figures/example1dcmotor.png" alt="bond graph of an ideal dc motor"/>

*A bond graph representation of an ideal DC motor.*
</center>
```latex
\begin{tikzpicture}
    \node (V) at (0,0) [bge={\mathbb{MS}e}{},pin={west:$ V_{abc} $}];
    \node (i) [right=of V,bge={1}{},label={300:$ i_{abc} $}];
    \node (Rs) [above=of i,bge={R}{},pin={north:$ R_s $}];
    \node (Ls) [below=of i,bge={L}{},pin={south:$ L_s $}];
    \node (Kl) [right=of i,bge={MGY}{},pin={south:$ K\Lambda(\theta_e) $}];
    \node (mx) [right=of Kl,mux={mux outputs=3}];
    \node (we) [right=of mx,bge={1}{},label={300:$ \omega_e $}];
    \node (1p) [right=of we,bge={TF}{},pin={south:$ \frac{1}{p} $}];
    \node (wm) [right=of 1p,bge={1}{},label={300:$ \omega_m $}];
    \node (Dr) [above=of wm,bge={R}{},pin={north:$ D_r $}];
    \node (Jr) [below=of wm,bge={I}{},pin={south:$ J_r $}];

    \begin{scope}[bond={multi}]
        \draw[bond={effort out}] (V) -- (i);
        \draw[bond={effort in}] (i) -- (Rs);
        \draw[bond={effort out}] (i) -- (Ls);
        \draw[bond={effort in}] (i) -- (Kl);
        \draw[bond={effort out}] (Kl) -- (mx.input 1);
    \end{scope}

    \foreach \i in {1,2,3}{
        \draw[bond={effort out}] (mx.output \i) -- ++ (0.8,0) -- (we);
    }
    \draw[bond={effort out}] (we) -- (1p);
    \draw[bond={effort out}] (1p) -- (wm);
    \draw[bond={effort out}] (wm) -- (Jr);
    \draw[bond={effort in}] (wm) -- (Dr);
\end{tikzpicture}
```

<center>
<img src="/figures/example2dcmotor3.png" alt="bond graph of a three phase dc motor">

*A bond graph representation of a three phase DC motor.*
</center>

# Alternatives
Other packages to draw bond graphs using Ti*k*z and/or PGF are the [bondgraph] package by Jean-François Dupuis, and the [bondgraphs] package by Geert Folkertsma. The former has no documentation and just a brief example file. The latter is well documented, but due to some drawing peculiarities, the resulting bond graphs are not always beautiful, especially when using multi bonds.

This package is based on the [bondgraphs] package and attempts to cover all its functionality. However it currently does not (yet) cover all functions. It does provide some additional functionality.

## Functionality comparison
The following table shows a function comparison between the three packages.

Function                                 | [bondgraph]       | [bondgraphs]        | [xbondgraphs]
-----------------------------------------|:------------------:|:------------------:|:------------------:
Automatic arrow barb direction           | :x:                | :heavy_check_mark: | :heavy_check_mark:
Single bond drawings                     | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
In-line bonds and elements               | :x:                | :heavy_check_mark: | :heavy_check_mark:
Multi bond drawings                      | :x:                | :white_check_mark: | :heavy_check_mark:
Power (de-)mux element                   | :x:                | :x:                | :heavy_check_mark:
Multi-segment bonds                      | :question:         | :x:                | :heavy_check_mark:
Curly bond barb                          | :heavy_check_mark: | :heavy_check_mark: | :x:
Optional colon between element and label | :x:                | :x:                | :heavy_check_mark:


This example shows a very simple multi-bond when using both the [bondgraphs] and the [xbondgraphs] package. You can clearly see the drawing peculiarities mentioned before in the focused parts of the drawing.

<center>
<img src="/figures/bondgraphs_example.png" alt="bondgraphs example" width="350"/>

*A bond graph with multi-bonds using the [bondgraphs] package.*
</center>

Using the [xbondgraphs] package, the resulting bond graph is more concise and does not show similar peculiarities.

<center>
<img src="/figures/xbondgraphs_example.png" alt="xbondgraphs example" width="350"/>

*A bond graph with multi-bonds using the [xbondgraphs] package.*
</center>



&copy; 2018 M.J.W. Snippe

[bondgraph]: https://ctan.org/pkg/bondgraph
[bondgraphs]: https://ctan.org/pkg/bondgraphs
[xbondgraphs]: https://github.com/MaxSnippe/xbondgraphs
