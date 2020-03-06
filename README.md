![VeRyPy](doc/logo.png)

<!-- ```
V  R  P
Ve Ry Py
 easy
``` -->

<!-- TODO: check and update these badges from shield.io and travis-ci.org -->
<!-- TODO: upload to PyPI -->
<!-- [![PyPI version](https://badge.fury.io/py/flair.svg)](https://badge.fury.io/py/flair) -->
<!-- [![GitHub Issues](https://img.shields.io/github/issues/zalandoresearch/flair.svg)](https://github.com/zalandoresearch/flair/issues) -->
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
<!-- TODO: https://www.smartfile.com/blog/testing-python-with-travis-ci/ -->
<!-- [![Travis](https://img.shields.io/travis/zalandoresearch/flair.svg)](https://travis-ci.org/zalandoresearch/flair) -->

VeRyPy is an **easy** to use **Python** library of classical Capacitated **Vehicle Routing Problem** (CVRP) algorithms. The enclosed implemented algorithms can also be used to solve travelling salesman problems (TSP). The code files are very loosely coupled so that you can take just the algorithms or the functionality you need in your studies or in your research project. 

## Introduction

Compared to the existing heuristic and metaheuristic open source VRP libraries such as the [VRPH](https://projects.coin-or.org/VRPH), [Google OR-Tools](https://developers.google.com/optimization/), and others, the focus in VeRyPy has been in reusability of the code and in replicating the existing results from the literature. There is no architecture astronautery in VeRyPy - instead, the focus is on the Python functions implementing the algorithms. The lightness of the framework is very much intentional as many existing libraries are complex beasts to reason about and understand which limits their use in a more exploratory setting. However, please note that the limitations of VeRyPy are also related to the main target of replication: it is not the fastest, the most sophisticated, nor the most effective library for solving these problems. But, if you are looking for something simple, VeRyPy might be just the perfect fit.

An ensemble of relatively simple heuristics can be an effective and robust way to solve practical problems and learning on the effectivity of different approaches before rolling out a more specialized and sophisticated algorithm. Furthermore, the quality of the solutions produced by the state-of-the-art metaheuristics depend on the quality of the initial solutions and VeRyPy can be used to produce a varied set of initial solutions for these more advanced methods.

## Features

* Implementations of 15 CVRP heuristics that are:
  * deterministic,
  * classical (from 60ies to mid 90ies, well-cited),
  * constructive (as opposed to improvement heuristics),
  * the correctness of implementation is shown through replication of the original results,
  * tested on a comprehensive set of 454 well-known CVRP benchmark instances.
  * For a full list, see [the heuristics list with references](#implemented-heuristics-with-references).
* Collection of local search heuristics:
  * intra route: 2-opt, 3-opt, relocate, exchange
  * inter route: insert, 2-opt*, 3-opt*<sup>1</sup>, one-point-move, two-point-swap, redistribute, chain
* Wrappers for [LKH](http://akira.ruc.dk/~keld/research/LKH/), [ACOTSP](http://www.aco-metaheuristic.org/aco-code/public-software.html), and [Gurobi TSP](https://www.gurobi.com/documentation/8.1/examples/tsp_py.html) solvers
* Command Line user Interface (CLI) for using the library and the separate algorithms
* Integration, replication, and even some unit tests
* Imports TSPLIB compatible CVRP and TSP files
* Exports VRPH compatible solutions 
* Visualizer for many of the 15 heuristics 
* Most of the algorithms are able to solve CVRP instances up to 1000 customers in under an hour
* The fastest algorithms can tackle problems with over 10 000 customers in a reasonable time

<sup>1</sup> In fact, due to its complexity, this just might be the only open source implementation of the 3-opt* operation out there.

<!-- TODO: animated gifs of the heuristics here -->
<!-- 
## Performance
TODO: Comparison to SOTA.
      e.g., https://www.localsolver.com/benchmarkcvrp.html
TODO: Replication results.
TODO: Time complexity curves from the paper
-->

![Gillet & Miller Sweep heuristic solving a 22 customer problem instance from Gaskell](doc/GM_sweep_anim_02-Gaskell2-E023-k5g_ccw.gif)
*Example of Gillet & Miller Sweep heuristic solving a 22 customer problem instance from Gaskell*

## Quick Start

The command line use assumes TSPLIB formatted files (assuming VeRyPy is in your PYTHONPATH):
```bash
$ python -O VeRyPy.py -a all E-n51-k5.vrp
```

> Note: running with `python -O` entirely disables `__debug__` and logging.

This simple Python code illustrates the API usage:
```python
import cvrp_io
from classic_heuristics.parallel_savings import parallel_savings_init
from util import sol2routes

E_n51_k5_path = r"E-n51-k5.vrp"

problem = cvrp_io.read_TSPLIB_CVRP(E_n51_k5_path)

solution = parallel_savings_init(
    D=problem.distance_matrix, 
    d=problem.customer_demands, 
    C=problem.capacity_constraint)

for route_idx, route in enumerate(sol2routes(solution)):
    print("Route #%d : %s"%(route_idx+1, route))
```

<!-- TODO: Make sure it works -->

<!-- TODO: A more comprehensive reference documentation can be found [here](/doc/). -->

### Dependencies and Installation

VeRyPy requires Python 2.7, NumPy, and SciPy. For CLI use you also need `natsort` from PyPI and some algorithms have additional dependencies: CMT79-2P, FR76-1PLT, GM74-SwRI and WH72-SwLS require `orderedset` from PyPI; MJ76-INS needs `llist` from PyPI; and FR76-1PLT, FG81-GAP, and DV89-MM require Gurobi with `gurobipy`. By default Be83-RFCS, SG82-LR3OPT, and Ty68-NN use LKH to solve TSPs, but they can be configured to use any other TSP solver (such as the internal one) if these external executables are not available.

<!-- TODO: insert dependency / object diagram here-->

## Contributing and Contacting

Please consider contributing to VeRyPy. But, if this library does not meet your needs, please check the alternatives: [VRPH](https://projects.coin-or.org/VRPH), [Google OR-Tools](https://developers.google.com/optimization/). In order to avoid reinventing (rewriting?) the wheel, please fight the NIH and consider the aforementioned options before rolling out your own VRP/TSP library.

All contributions including, but not limited to, improving the implemented algorithms, implementing new classical heuristics or metaheuristics, reporting bugs, fixing bugs, improving documentation, writing tutorials, or improving the usability of the library are welcome. However, please note that any contributions you make will be under the MIT Software License. Even if you are not that much into coding please note that blogging, tweeting, and just plain talking about VeRyPy will help the project to grow and prosper.

When contributing:
* Report bugs using Github issues, and, while doing so, be specific and show how to reproduce the bug
* Use a consistent Pythonic coding style (PEP 8)
* The comments and docstrings have to be written in NumPy Style

Feature requests can be made, but there is no guarantee that they will be addressed. For a more complex use cases one can contact Jussi Rasku who is the main author of this library: <jussi.rasku@jyu.fi> .

If you are itching to get started, please refer to the todo list below:

1. Package project : https://packaging.python.org/tutorials/packaging-projects/
2. Transform all docstrings to NumPy format 
3. Generate restructured documentation 	https://pythonhosted.org/sphinxcontrib-restbuilder/
https://www.sphinx-doc.org/en/master/usage/quickstart.html

## Citing VeRyPy

If you find VeRyPy useful in your research and use it to produce results for your publications please consider citing it as:

> Rasku J, Musliu N, Kärkkäinen T. Meta-Survey and Implementations of Classic Capacitated Vehicle Routing Heuristics with Reproductions of Earlier Results. Manuscript. University of Jyväskylä, Finland.


## Implemented Heuristics with References

> [cmt](#cmt)

**CMT79-2P** : Christofides, Mingozzi & Toth (1979) two phase heuristic.

> [gap](#gap)

**FJ81-GAP** : Fisher & Jaikumar (1981) generalized assignment problem (GAP) heuristic. *Requires Gurobi.*

> [gm](#gm)

**GM74-SwRI** : Gillett & Miller (1974) Sweep algorithm with emering route improvement step.

> [gpl](#gpl)

**Ga67-PS|𝜋𝜆** : Parallel savings algorithm with Gaskell (1967) with the 𝜋 and 𝜆 criteria.

> [gps](#gps)

**Pa88-PS|G2P** : Paessens (1988) parametrized parallel savings algorithm.

> [ims](#ims)

**HP76-PS|IMS** : Holmes & Parker (1976) parallel savings supression algorithm.

> [lr3o](#lr3o)

**SG84-LR3OPT** : Stewart & Golden (1984) 3-opt* heuristic with Lagrangian relaxations.

> [mbsa](#mbsa)

**DV89-MM** : Desrochers and Verhoog (1989) maximum matching problem solution based savings algorithm. *Requires Gurobi.*

> [mj](#mj)

**MJ76-INS** : Mole & Jameson (1976) sequential cheapest insertion heuristic with a route improvement phase.

> [pi](#pi)

**vB94-PI** : parallel insertion heuristic as described by van Breedam (1994, 2002).

> [pn](#pn)

**vB95-PNN** : Parallel Nearest Neighbor construction heuristic.

> [ps](#ps)

**CW64-PS** : Clarke & Wright (1964) parallel savings algorithm.

> [ptl](#ptl)

**FR76-1PTL** : Foster & Ryan (1976) Petal set covering algorithm. *Requires Gurobi.*

> [rfcs](#rfcs) 

**Be83-RFCS** : Route-first-cluster-second heuristic of Beasley (1983).

> [si](#si)

**vB94-SI** : Sequential cheapest insertion heuristic without local search (van Breedam 1994, 2002).

> [vB95](#vB95)-SNN

`sn` : Sequential Nearest Neighbor construction heuristic as described by van Breedam (1994).

> [We64-SS](#We64-SS)

`ss` : Webb (1964) sequential savings algorithm.

> Sweep

`swp` : Sweep algorithm without any route improvement heuristics.

> [Ty68-NN](#Ty68-NN)

`ty` : Tyagi (1968) Nearest Neighbor construction heuristic. 

> [WH72-SwLS](#WH72-SwLS)

`wh` : Sweep heuristic with Wren and Holliday (1972) improvement procedures.

> <a name="vB95">vB95</a>: Van Breedam, A. (1994). An Analysis of the Behavior of Heuristics for the Vehicle Routing Problem for a Selectrion of Problems with Vehicle-related, Customer-related, and Time-related Constraints. PhD thesis, Faculty of Applied Economics, University of Antwerp, Belgium - RUCA. 
> 
> and
> 
> Van Breedam, A. (2002). A parametric analysis of heuristics for the vehicle routing problem with side-constraints. European Journal of Operational Research, 137(2):348-370.


> <a name="We64-SS">We64-SS</a>: Webb, M. (1964). A study in transport routing. Glass Technology, 5:178-181.

> <a name="Ty68-NN">Ty68-NN</a>: Tyagi, M. S. (1968). A practical method for the truck dispatching problem. Journal of the Operations Research Society of Japan, 10:76-92.

> <a name="WH72-SwLS">WH72-SwLS</a>: Wren, A. and Holliday, A. (1972). Computer scheduling of vehicles from one or more depots to a number of delivery points. Journal of the Operational Research Society, 23(3):333-344.

<!-- ## References
TODO: links -->





## License

VeRyPy is licensed under the MIT license.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.





 
