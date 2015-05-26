# TDAmapper: Topological Data Analysis using Mapper


## Description

An R package for using discrete Morse theory to analyze a data set using the Mapper algorithm described in:

> G. Singh, F. Memoli, G. Carlsson (2007).  Topological Methods for the Analysis of High Dimensional Data Sets and 3D Object Recognition, Point Based Graphics 2007, Prague, September 2007.


## Installation

To install the latest version of this R package directly from github:

    install.packages("devtools")
    library(devtools)
    devtools::install_github("paultpearson/TDAmapper")
    library(TDAmapper)

To install from Github you might need: 

- **Windows:** Rtools (http://cran.r-project.org/bin/windows/Rtools/)
- **OS X:** xcode (from the app store)
- **Linux:** apt-get install r-base-dev (or similar).

To install the stable version of this R package from CRAN:

    install.packages("TDAmapper")

	
## Example 1: Using mapper1D to identify a figure 8

    # The fastcluster package is not necessary.  By loading the
	# fastcluster package, the fastcluster::hclust() function 
	# automatically replaces the slower stats::hclust() function
	# whenever hclust() is called.
    install.packages("fastcluster") 
    require(fastcluster) 

	m1 <- mapper1D(
		distance_matrix = dist(data.frame( x=2*cos(0.5*(1:100)), y=sin(1:100) )),
		filter_values = 2*cos(0.5*(1:100)),
		num_intervals = 10,
		percent_overlap = 50,
		num_bins_when_clustering = 10)

	# The igraph package is necessary to view simplicial complexes
    # (undirected graph) resulting from mapper1D().
    install.packages("igraph") 
    library(igraph)

	g1 <- graph.adjacency(m1$adjacency, mode="undirected")
	plot(g1, layout = layout.auto(g1) )


## Example 2: Using mapper2D to identify an oval as S^1

	m2 <- mapper2D(
		distance_matrix = dist(data.frame( x=2*cos(1:100), y=sin(1:100) )),
		filter_values = list( 2*cos(1:100), sin(1:100) ),
		num_intervals = c(5,5),
		percent_overlap = 50,
		num_bins_when_clustering = 10)

	g2 <- graph.adjacency(m2$adjacency, mode="undirected")
	plot(g2, layout = layout.auto(g2) )


## Example 3: using mapper1D to identify two independent spirals as two line segments

    # sample points from two intertwined spirals
    set.seed("1")
    t <- runif(100, min=1, max=6.3) # theta
    X <- data.frame( x = c( t*cos(t), -t*cos(t) ), y = c( t*sin(t), -t*sin(t) ) )
    d <- dist(X)
    plot(X[,1], X[,2])
        
    filter <- X[,2] # height projection
    num_intervals <- 10
    percent_overlap <- 50
    num_bins_when_clustering <- 10

	m3 <- mapper1D(
		distance_matrix = d, 
		filter_values = filter,	
		# num_intervals = 10, # use default
		# percent_overlap = 50, # use default
		# num_bins_when_clustering = 10 # use default
		)
        
    g3 <- graph.adjacency(m3$adjacency, mode="undirected")
    plot(g3, layout = layout.auto(g3) )

	
# Author's notes to himself (you can ignore this section)

These are some notes by the package author to himself about the package creation process.  Locally installing, building, and checking the package can be done with:

    R>setwd("C:/research/TDAmapper")
    R>devtools::document()
	C:\research>"C:\Program Files\R\R-3.2.0\bin\R.exe" CMD build TDAMapper
    C:\research>"C:\Program Files\R\R-3.2.0\bin\R.exe" CMD check TDAmapper_0.0.0.9000.tar.gz --as-cran
    C:\research>"C:\Program Files\R\R-3.2.0\bin\R.exe" CMD INSTALL TDAmapper_0.0.0.9000.tar.gz
	C:\research>"C:\Program Files\R\R-3.2.0\bin\R.exe" CMD Rd2pdf TDAmapper
	
# License

The mapper1D function by Paul Pearson is a cleaned-up, modified, and ported version of the Mapper code by Daniel Muellner and Gurjeet Singh originally written for Matlab.  What follows is the copyright notice included in the Matlab code written by Muellner, which was based on code written by Singh.

    This is a cleaned-up and modified version of the Mapper code by
    Gurjeet Singh. It also corrects two bugs which are present in the
    original Mapper code.
    (c) 2010 Daniel Muellner, muellner@math.stanford.edu
    Copyright: As far as Daniel Muellner's contributions are concerned,
    this code is published under the GNU General Public License v3.0
    (see http://www.gnu.org/licenses/gpl.html). For scientific citations,
    please refer to my home page http://math.stanford.edu/~muellner. If
    you visit this page in the future, chances are high that you will find
    a Python library with improved, largely extended and freely
    distributable Mapper code there.
    Since the present code is based on Gurjeet Singh's original code,
    please also respect his copyright message.
    Below is the original copyright message:
    Mapper code -- (c) 2007-2009 Gurjeet Singh
    This code is provided as is, with no guarantees except that
    bugs are almost surely present.  Published reports of research
    using this code (or a modified version) should cite the
    article that describes the algorithm:
      G. Singh, F. Memoli, G. Carlsson (2007).  Topological Methods for
      the Analysis of High Dimensional Data Sets and 3D Object
      Recognition, Point Based Graphics 2007, Prague, September 2007.
    Comments and bug reports are welcome.  Email to
    gurjeet@stanford.edu.
    I would also appreciate hearing about how you used this code,
    improvements that you have made to it, or translations into other
    languages.
    You are free to modify, extend or distribute this code, as long
    as this copyright notice is included whole and unchanged.
