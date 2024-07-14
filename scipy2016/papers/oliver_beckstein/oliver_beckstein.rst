.. -*- mode: rst; mode: visual-line; fill-column: 9999; coding: utf-8 -*-

:author: Richard J. Gowers
:email: richardjgowers@gmail.com
:institution: University of Manchester, Manchester, UK
:institution: University of Edinburgh, Edinburgh, UK
:equal-contributor:

:author: Max Linke
:email: max.linke@biophys.mpg.de
:institution: Max Planck Institut für Biophysik, Frankfurt, Germany
:equal-contributor:

:author: Jonathan Barnoud
:email: j.barnoud@rug.nl
:institution: University of Groningen, Groningen, The Netherlands
:equal-contributor:

:author: Tyler J. E. Reddy
:email: tyler.reddy@bioch.ox.ac.uk
:institution: University of Oxford, Oxford, UK

:author: Manuel N. Melo
:email: m.n.melo@rug.nl
:institution: University of Groningen, Groningen, The Netherlands

:author: Sean L. Seyler
:email: slseyler@asu.edu
:institution: Arizona State University, Tempe, Arizona, USA

:author: Jan Domański
:email: jan.domanski@bioch.ox.ac.uk
:institution: University of Oxford, Oxford, UK

:author: David L. Dotson
:email: dldotson@asu.edu
:institution: Arizona State University, Tempe, Arizona, USA

:author: Sébastien Buchoux
:email: sebastien.buchoux@u-picardie.fr
:institution: Université de Picardie Jules Verne, Amiens, France

:author: Ian M. Kenney
:email: Ian.Kenney@asu.edu
:institution: Arizona State University, Tempe, Arizona, USA


:author: Oliver Beckstein
:email: oliver.beckstein@asu.edu
:institution: Arizona State University, Tempe, Arizona, USA
:corresponding:

:video: https://youtu.be/zVQGFysYDew

:bibliography: ``mdanalysis``


.. STYLE GUIDE
.. ===========
.. see https://github.com/MDAnalysis/scipy_proceedings/wiki
.. .
.. Writing
..  - use present tense
.. .
.. Formatting
..  - restructured text
..  - hard line breaks after complete sentences (after period)
..  - paragraphs: empty line (two hard line breaks)
.. .
.. Workflow
..  - use PRs (keep them small and manageable)


.. definitions (like \newcommand)

.. |Calpha| replace:: :math:`\mathrm{C}_\alpha`


-------------------------------------------------------------------------------------
MDAnalysis: A Python Package for the Rapid Analysis of Molecular Dynamics Simulations
-------------------------------------------------------------------------------------

.. class:: abstract

MDAnalysis (http://mdanalysis.org) is a library for structural and temporal analysis of molecular dynamics (MD) simulation trajectories and individual protein structures.
MD simulations of biological molecules have become an important tool to elucidate the relationship between molecular structure and physiological function.
Simulations are performed with highly optimized software packages on HPC resources but most codes generate output trajectories in their own formats so that the development of new trajectory analysis algorithms is confined to specific user communities and widespread adoption and further development is delayed.
MDAnalysis addresses this problem by abstracting access to the raw simulation data and presenting a uniform object-oriented Python interface to the user.
It thus enables users to rapidly write code that is portable and immediately usable in virtually all biomolecular simulation communities.
The user interface and modular design work equally well in complex scripted work flows, as foundations for other packages, and for interactive and rapid prototyping work in IPython_  / Jupyter_ notebooks, especially together with molecular visualization provided by nglview_ and time series analysis with pandas_.
MDAnalysis is written in Python and Cython_ and uses NumPy_ arrays for easy interoperability with the wider scientific Python ecosystem.
It is widely used and forms the foundation for more specialized biomolecular simulation tools.
MDAnalysis is available under the GNU General Public License v2.

.. _IPython: http://ipython.org/
.. _Jupyter: http://jupyter.org/
.. _nglview: https://github.com/arose/nglview
.. _pandas: http://pandas.pydata.org/
.. _NumPy: http://www.numpy.org
.. _Cython: http://cython.org/
.. _matplotlib: http://matplotlib.org
.. _MayaVi: http://code.enthought.com/projects/mayavi/
.. _distributed: https://github.com/dask/distributed
.. _fireworks: https://github.com/materialsproject/fireworks
.. _MDSynthesis: https://github.com/datreant/MDSynthesis
.. _datreant: http://datreant.org

.. class:: keywords

   molecular dynamics simulations, science, chemistry, physics, biology


Introduction
------------

Molecular dynamics (MD) simulations of biological molecules have become an important tool to elucidate the relationship between molecular structure and physiological function :cite:`Dror:2012cr,Orozco:2014dq`.
Simulations are performed with highly optimized software packages on HPC resources but most codes generate output trajectories in their own formats so that the development of new trajectory analysis algorithms is confined to specific user communities and widespread adoption and further development is delayed.
Typical trajectory sizes range from gigabytes to terabytes so it is typically not feasible to convert trajectories into a range of different formats just to use a tool that requires this specific format.
Instead, a framework is required that provides a common interface to raw simulation data.
Here we describe the MDAnalysis_ library :cite:`Michaud-Agrawal:2011fu` that addresses this problem by abstracting access to the raw simulation data.
MDAnalysis presents a uniform object-oriented Python interface to the user.
Since its original publication in 2011 :cite:`Michaud-Agrawal:2011fu`, MDAnalysis has been widely adopted and has undergone substantial changes.
Here we provide a short introduction to MDAnalysis and its capabilities and an overview over recent improvements.

MDAnalysis was initially inspired by MDTools_ for Python (J.C. Phillips, unpublished) and MMTK_ :cite:`Hinsen:2000kx`.
MDTools pioneered the key idea to use an extensible and object-oriented language, namely, Python, to provide a high-level interface for the construction and analysis of molecular systems for MD simulations.
MMTK became a tool kit to build MD simulation applications on the basis of a concise object model of a molecular system.
MDAnalysis was built on an object model similar to that of MMTK with a strong focus on providing universal high-level building blocks for the analysis of MD trajectories, but for a much wider range of formats than previously available.
MDAnalysis has been publicly available since January 2008 and is one of the longest actively maintained Python packages for the analysis of molecular simulations.
Since then many other packages have appeared that primarily function as libraries for providing access to simulation data from within Python.
Three popular examples are PyLOOS_ :cite:`Romo:2014bh`, mdtraj_ :cite:`McGibbon:2015aa`, and pytraj_ :cite:`Nguyen:2016aa`.
PyLOOS :cite:`Romo:2014bh` consists of Python bindings to the C++ LOOS_ library :cite:`Romo:2009zr`; in order to aid novice users, LOOS also provides about 140 small stand-alone tools that each focus on a single task.
mdtraj :cite:`McGibbon:2015aa` is similar to MDAnalysis in many aspects but focuses even more on being a light-weight building block for other packages; it also includes a number of innovative performance optimizations.
pytraj :cite:`Nguyen:2016aa` is a versatile Python frontend to the popular and powerful ``cpptraj`` tool :cite:`Roe:2013zr` and is particularly geared towards users of the Amber MD package :cite:`Case:2005uq`.
These three packages and MDAnalysis have in common that they are built on an object model of the underlying data (such as groups of particles or a trajectory), use compiled code in C, C++ or Cython to accelerate time critical bottlenecks, and have a "Pythonic" user interface. 
LOOS and MDAnalysis share a similar object-oriented philosophy in their user interface design.
In contrast, mdtraj and pytraj expose a functional user interface. 
Both approaches have advantages and the existence of different "second generation" Python packages for the analysis of MD simulations provides many good choices for users and a fast moving and stimulating environment for developers.

.. _MDAnalysis: http://mdanalysis.org
.. _MDTools: http://www.ks.uiuc.edu/Development/MDTools/Python/
.. _MMTK: http://dirac.cnrs-orleans.fr/MMTK/
.. _PyLOOS: http://loos.sourceforge.net/
.. _LOOS: http://loos.sourceforge.net/
.. _mdtraj: http://mdtraj.org/
.. _pytraj: https://github.com/Amber-MD/pytraj


Overview
--------

MDAnalysis is specifically tailored to the domain of molecular simulations, in particularly in biophysics, chemistry, and biotechnology as well as materials science.
The user interface provides physics-based abstractions (e.g., atoms, bonds, molecules) of the data that can be easily manipulated by the user.
It hides the complexity of accessing data and frees the user from having to implement the details of different trajectory and topology file formats (which by themselves are often only poorly documented and just adhere to certain community expectations that can be difficult to understand for outsiders).
MDAnalysis currently supports more than 25 different file formats and covers the vast majority of data formats that are used in the biomolecular simulation community, including the formats required and produced by the most popular packages such as NAMD :cite:`Phillips:2005ek`, Amber :cite:`Case:2005uq`, Gromacs :cite:`Abraham:2015aa`, CHARMM :cite:`Brooks:2009pt`, LAMMPS :cite:`Plimpton:1995rw`, DL_POLY :cite:`Todorov:2006kx`, HOOMD :cite:`Glaser:2015ys` as well as the Protein Data Bank PDB format :cite:`Berman:2000aa` and various other specialized formats.

Since the original publication :cite:`Michaud-Agrawal:2011fu`, improvements in speed and data structures make it now possible to work with terabyte-sized trajectories containing up to ~10 million particles.
MDAnalysis also comes with specialized analysis classes in the ``MDAnalysis.analysis`` module that are unique to MDAnalysis such as *LeafletFinder* (in the ``leaflet`` module), a graph-based algorithm for the analysis of lipid bilayers :cite:`Michaud-Agrawal:2011fu`, or *Path Similarity Analysis* (``psa``) for the quantitative comparison of macromolecular conformational changes :cite:`Seyler:2015fk`.


Code base
~~~~~~~~~

MDAnalysis is written in Python and Cython_ with about 42k lines of code and 24k lines of comments and documentation.
It uses NumPy_ arrays :cite:`Vanderwalt2011` for easy interoperability with the wider scientific Python ecosystem.
Although the primary dependency is NumPy_, other Python packages such as netcdf4_  and BioPython_ :cite:`Hamelryck:2003fv` also provide specialized functionality to the core of the library (Figure :ref:`fig:structure`).

.. figure:: figs/mdanalysis_structure.pdf

   Structure of the MDAnalysis package.
   MDAnalysis consists of the *core* with the *Universe* class as the primary entry point for users.
   The ``MDAnalysis.analysis`` package contains independent modules that make use of the core to implement a wide range of algorithms to analyze MD simulations.
   The ``MDAnalysis.visualization`` package contains a growing number of tools that are specifically geared towards calculating visual representations such as, for instance, streamlines of molecules. :label:`fig:structure`


Availability
~~~~~~~~~~~~

MDAnalysis is available in source form under the GNU General Public License v2 from GitHub as `MDAnalysis/mdanalysis`_, and as PyPi_ and conda_ packages.
The documentation_ is extensive and includes an `introductory tutorial`_.


Development process
~~~~~~~~~~~~~~~~~~~

The development community is very active with more than five active core developers and many community contributions in every release.
We use modern software development practices :cite:`Wilson:2014aa,Stodden:2014tg` with continuous integration (provided by `Travis CI`_) and an extensive automated test suite (containing over 3500 tests with >92% coverage for our core modules).
Development occurs on GitHub_ through pull requests that are reviewed by core developers and other contributors, supported by the results from the automated tests, test coverage reports provided by Coveralls_, and QuantifiedCode_ code quality reports.
Users and developers communicate extensively on the `community mailing list`_ (*Google* groups) and the GitHub issue tracker; new users and developers are very welcome and most user contributions are eventually integrated into the code base.
The development and release process is transparent to users through open discussions and announcements and a full published commit history and changes.
Releases are numbered according to the `semantic versioning`_ convention so that users can immediately judge the impact of a new release on their existing code base, even without having to consult the ``CHANGELOG`` documentation.
Old code is slowly deprecated so that users have ample opportunity to update the code although we generally attempt to break as little code as possible.
When backwards-incompatible changes are inevitable, we provide tools (based on the Python standard library's *lib2to3*) to automatically refactor code or warn users of possible problems with their existing code.


.. _PyPi: https://pypi.python.org/pypi/MDAnalysis
.. _conda: https://anaconda.org/mdanalysis/dashboard
.. _community mailing list: https://groups.google.com/forum/#!forum/mdnalysis-discussion
.. _ENCORE: https://github.com/encore-similarity/encore
.. _ProtoMD: https://github.com/CTCNano/proto_md
.. _`ST-analyzer`: http://im.compbio.ku.edu/st-analyzer/
.. _introductory tutorial: http://www.mdanalysis.org/MDAnalysisTutorial/
.. _documentation: http://docs.mdanalysis.org
.. _`MDAnalysis/mdanalysis`: https://github.com/MDAnalysis/mdanalysis
.. _semantic versioning: http://semver.org
.. _netcdf4: http://unidata.github.io/netcdf4-python/
.. _BioPython: http://biopython.org/wiki/Biopython

.. _Travis CI: http://travis-ci.org/
.. _GitHub: http://github.com
.. _Coveralls: https://coveralls.io/
.. _QuantifiedCode: https://www.quantifiedcode.com


Basic usage
-----------

The core object in MDAnalysis is the Universe which acts as a nexus for accessing all data contained within a simulation.
It is initialized by passing the file names of the topology and trajectory files, with a multitude of different formats supported in these roles.
The topology acts as a description of all the particles in the system while the trajectory describes their behavior over time.

.. show loading a Universe and creating basic selections
.. check that this selection makes chemical sense!
.. code-block:: python

   import MDAnalysis as mda

   # Create a Universe based on simulation results
   u = mda.Universe('topol.tpr', 'traj.trr')

   # Create a selection of atoms to work with
   ag = u.atoms.select_atoms('backbone')

The select_atoms method allows for AtomGroups to be created using a human readable syntax which allows queries according to properties, logical statements and geometric criteria.

.. more selection examples, these include
.. logic operations (NOT AND)
.. geometry based (AROUND)
.. other group based (GROUP)
.. TODO (maybe): brackets, OR, cylinder/sphere?
.. code-block:: python

   # Select all solvent within a set distance from protein atoms
   ag = u.select_atoms('resname SOL and around 5.0 protein')

   # Select all heavy atoms in the first 20 residues
   ag = u.select_atoms('resid 1:20 and not prop mass < 10.0')

   # Use a preexisting AtomGroup as part of another selection
   sel1 = u.select_atoms('name N and not resname MET')
   sel2 = u.select_atoms('around 2.5 group Nsel', Nsel=sel1)

   # Perform a selection on another AtomGroup
   sel1 = u.select_atoms('around 5.0 protein')
   sel2 = sel1.select_atoms('type O')

The AtomGroup acts as a representation of a group of particles, with the properties of these particles made available as NumPy arrays.

.. accessing data from an atomgroup
.. topology data
.. trajectory data
.. code-block:: python

   ag.names
   ag.charges
   ag.positions
   ag.velocities
   ag.forces

The data from MD simulations comes in the form of a trajectory which is a frame by frame description of the motion of particles in the simulation.
Today trajectory data can often reach sizes of hundreds of GB.
Reading all these data into memory is slow and impractical.
To allow the analysis of such large simulations on an average workstation (or even laptop) MDAnalysis will only load a single frame of a trajectory into memory at any time.

The trajectory data can be accessed through the trajectory attribute of a Universe.
Changing the frame of the trajectory object updates the underlying arrays that AtomGroups point to.
In this way the positions attribute of an AtomGroup within the iteration over a trajectory will give access to the positions at each frame.
Through this approach only a single frame of data is present in memory at any time, allowing for large data sets, from half a million particles to tens of millions (see also section `Analysis of large systems`_), to be dissected with minimal resources.

.. show working with the trajectory object to access the time data
.. code-block:: python

   # the trajectory is an iterable object
   len(u.trajectory)

   # seek to a given frame
   u.trajectory[72]
   # iterate through every 10th frame
   for ts in u.trajectory[::10]:
       ag.positions

In some cases it is necessary to access frames of trajectories in a random access pattern or at least be able to rapidly access a starting frame anywhere in the trajectory.  
Examples for such usage are the calculation of time correlation functions, skipping of frames (as in the iterator ``u.trajectory[5000::1000]``), or parallelization over trajectory blocks in a map/reduce pattern :cite:`Tu:2008dq`. If the underlying trajectory reader only implements linear sequential reading from the beginning, searching for specific frames becomes extremely inefficient, effectively prohibiting random access to time frames on disk.
Many trajectory formats suffer from this shortcoming, including the popular Gromacs XTC and TRR formats, but also commonly used multi-frame PDB files and other text-based formats such as XYZ.
LOOS :cite:`Romo:2009zr` implemented a mechanism by which the trajectory was read once on loading and frame offsets on disk were computed that could be used to directly seek to individual frames.
Based on this idea, MDAnalysis implements a fast frame scanning algorithm for TRR and XTC files and also saves the offsets to disk (as a compressed NumPy array).
When a trajectory is loaded again then instead of reading the whole trajectory, only the persistent offsets are read (provided they have not become stale as checked by conservative criteria such as changes in file name, modification time, and size of the original file, which are all saved with the offsets).
In cases of terabyte-sized trajectories, the persistent offset approach can save hundreds of seconds for the initial loading of the ``Universe`` (after an initial one-time cost of scanning the trajectory).
Current development work is extending the persistent offset scheme to all trajectory readers, which will provide random access for all trajectories in a completely automatic and transparent manner to the user.



Example: Per-residue RMSF
~~~~~~~~~~~~~~~~~~~~~~~~~

As a complete example consider the calculation of the |Calpha| root mean square fluctuation (RMSF) :math:`\rho_i` that characterizes the mobility of a residue :math:`i` in a protein:

.. math::
   :label: eq:RMSF
   
   \rho_i = \sqrt{\left\langle\left(\mathbf{x}_i(t) - \langle\mathbf{x}_i\rangle\right)^2\right\rangle}

The code in Figure :ref:`fig:rmsf-example` A shows how MDAnalysis in combination with NumPy can be used to implement Eq. :ref:`eq:RMSF`.
The topology information and the trajectory are loaded into a ``Universe`` instance; |Calpha| atoms are selected with the MDAnalysis selection syntax and stored as the ``AtomGroup`` instance ``ca``.
The main loop iterates through the trajectory using the MDAnalysis trajectory iterator.
The coordinates of all selected atoms become available in a NumPy array ``ca.positions`` that updates for each new time step in the trajectory.
Fast operations on this array are then used to calculate variance over the whole trajectory.
The final result is plotted with matplotlib_ :cite:`Hunter:2007aa` as the RMSF over the residue numbers, which are conveniently provided as an attribute of the ``AtomGroup`` (Figure :ref:`fig:rmsf-example` B).


.. figure:: figs/rmsf_Example.pdf

   Example for how to calculate the root mean square fluctuation (RMSF) for each residue in a protein with MDAnalysis and NumPy. **A**: Based on the input simulation data (topology and trajectory in the Gromacs format (TPR and XTC), MDAnalysis makes coordinates of the selected |Calpha| atoms available as NumPy arrays. From these coordinates, the RMSF is calculated by averaging over all frames in the trajectory. The RMSF is then plotted with matplotlib_. The algorithm to calculate the variance in a single pass is due to Welford :cite:`Welford:1962aa`. **B**: |Calpha| RMSF for each residue. :label:`fig:rmsf-example`

The example demonstrates how the abstractions that MDAnalysis provides enable users to write concise code where the computations on data are cleanly separated from the task of extracting the data from the simulation trajectories.
These characteristics make it easy to rapidly prototype new algorithms.
In our experience, most new analysis algorithms are developed by first prototyping a simple script (like the one in Figure :ref:`fig:rmsf-example`), often inside a Jupyter_ notebook (see section `Interactive Use and Visualization`_).
Then the code is cleaned up, tested and packaged into a module.
In section `Analysis Module`_, we describe the analysis code that is included as modules with MDAnalysis.


.. _`Interactive Use and Visualization`:

Interactive use and visualization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The high level of abstraction and the pythonic API, together with comprehensive Python doc strings, make MDAnalysis well suited for interactive and rapid prototyping work in IPython_ :cite:`Perez2007` and Jupyter_ notebooks.
It works equally well as an interactive analysis tool, especially with Jupyter notebooks, which then contain an executable and well-documented analysis protocol that can be easily shared and even accessed remotely.
Universes and AtomGroups can be visualized in Jupyter notebooks using nglview_, which interacts natively with the MDAnalysis API (Figure :ref:`fig:nglview`).

.. figure:: figs/nglview.png

   MDAnalysis can be used with nglview_ to directly visualize molecules and trajectories in Jupyter_ notebooks. The adenylate kinase (AdK) protein from one of the included test trajectories is shown. :label:`fig:nglview`.

Other Python packages that have become extremely useful in notebook-based analysis work flows are pandas_  :cite:`McKinney2010` for rapid analysis of time series analysis, distributed_ :cite:`Rocklin:2015aa` for simple parallelization, FireWorks_ :cite:`Jain:2015aa` for complex work flows, and MDSynthesis_ :cite:`Dotson:2016aa` for organizing, bundling and querying many simulations.


.. _`Analysis Module`:

Analysis module
---------------

In the ``MDAnalysis.analysis`` module we provide a large variety of standard analysis algorithms, like RMSD (root mean square distance) and RMSF (root mean square fluctuation) calculations, RMSD-optimized structural superposition :cite:`PuLiu_FastRMSD_2010`, native contacts :cite:`Best2013,Franklin2007`, or analysis of hydrogen bonds as well as unique algorithms, such as the *LeafletFinder* in ``MDAnalysis.analysis.leaflet`` :cite:`Michaud-Agrawal:2011fu` and *Path Similarity Analysis* (``MDAnalysis.analysis.psa``) :cite:`Seyler:2015fk`.
Historically these algorithms were contributed by various researchers as individual modules to satisfy their own needs but this lead to some fragmentation in the user interface.
We have recently started to unify the interface to the different algorithms with an `AnalysisBase` class.
Currently ``PersistenceLength``, ``InterRDF``, ``LinearDensity`` and ``Contacts`` analysis have been ported.
``PersistenceLength`` calculates the persistence length of a polymer, ``InterRDF`` calculates the pairwise radial distribution function inside of a molecule, ``LinearDensity`` generates a density along a given axis and ``Contacts`` analysis native contacts, as described in more detail below.
The API to these different algorithms is being unified with a common ``AnalysisBase`` class, with an emphasis on keeping it as generic and universal as possible so that it becomes easy to, for instance, parallelize analysis.
Most other tools hand the user analysis algorithms as black boxes.
We want to avoid that and allow the user to adapt an analysis to their needs.

The new ``Contacts`` class is a good example of a generic API that allows straightforward implementation of algorithms while still offering an easy setup for standard analysis types.
The ``Contacts`` class is calculating a contact map for atoms in a frame and compares it with a reference map using different metrics.
The used metric then decides which quantity is measured.
A common quantity is the fraction of native contacts, where native contacts are all atom pairs that are close to each other in a reference structure.
The fraction of native contacts is often used in protein folding to determine when a protein is folded.
For native contacts two major types of metrics are considered: ones based on differentiable functions :cite:`Best2013` and ones based on hard cut-offs  :cite:`Franklin2007` (which we set as the default implementation).
We have designed the API to choose between the two metrics and pass user defined functions to develop new metrics or measure other quantities.
This generic interface allowed us to implement a ":math:`q_1 q_2`" analysis :cite:`Franklin2007` on top of the ``Contacts`` class; :math:`q_1` and :math:`q_2` refer to the fractions of native contacts that are present in a protein structure relative to *two* reference states 1 and 2.
Below is an incomplete code example that shows how to implement a :math:`q_1 q_2` analysis, the default value for the *method* keyword argument is overwritten with a user defined method *radius_cut_q*.
A more detailed explanation can be found in the documentation.

.. code-block:: python

   def radius_cut_q(r, r0, radius):
       y = r <= radius
       return y.sum() / r.size

   contacts = Contacts(u, selection,
                       (first_frame, last_frame),
                       radius=radius,
                       method=radius_cut_q,
                       start=start, stop=stop,
                       step=step,
                       kwargs={'radius': radius})

This type of flexible analysis algorithm paired with a collection of base classes enables rapid and easy analysis of simulations as well as development of new ones.


Visualization module
--------------------

The new ``MDAnalysis.visualization`` name space contains modules that primarily produce visualizations of molecular systems.
Currently it contains functions that generate specialized streamline visualizations of lipid diffusion in membrane bilayers :cite:`C3FD00145H`.
In short, the algorithm decomposes any given membrane into a grid and tracks the displacement of lipids between different grid elements, emphasizing collective lipid motions.
Both 2D (``MDAnalysis.visualization.streamlines``) and 3D (``MDAnalysis.visualization.streamlines_3D``) implementations are available in MDAnalysis, with output shown in Figure :ref:`fig:streamlines`.
Sample input data files are available online from the Flows_ website along with the expected output visualizations.

.. figure:: figs/streamlines.pdf

   Visualization of the flow of lipids in a large bilayer membrane patch. **A**: 2D stream plot (produced with ``MDAnalysis.visualization.streamlines`` and plotted with matplotlib_ :cite:`Hunter:2007aa`). **B**: 3D stream plot, viewed down the :math:`z` axis onto the membrane (produced with ``MDAnalysis.visualization.streamlines_3D`` and plotted with MayaVi_ :cite:`Ramachandran:2011aa`). :label:`fig:streamlines`

.. _Flows: http://sbcb.bioch.ox.ac.uk/flows/MDAnalysis.html


Improvements in the internal topology data structures
-----------------------------------------------------

Originally MDAnalysis followed a strict object-oriented approach with a separate instance of an Atom object for each particle in the simulation data.
The AtomGroup then simply stored its contents as a list of these Atom instances.
With simulation data now commonly exceeding :math:`10^6` particles this solution did not scale well and so recently this design was overhauled to improve the scalability of MDAnalysis.

Because all Atoms have the same property fields (i.e. mass, position) it is possible to store this information as a single NumPy array for each property.
Now an AtomGroup can keep track of its contents as a simple integer array, which can be used to slice these property arrays to yield the relevant data.

Overall this approach means that the same number of Python objects are created for each Universe, with the number of particles only changing the size of the arrays.
This translates into a much smaller memory footprint (1.3 GB vs. 3.6 GB for a 10.1 M atom system), highlighting the memory cost of millions of simple Python objects.

This transformation of the data structures from an Array of Structs to a Struct of Arrays also better suits the typical access patterns within MDAnalysis.
It is quite common to compare a single property across many Atoms, but rarely are different properties within a single Atom compared.
Additionally, it is possible to utilize NumPy's faster indexing capabilities rather than using a list comprehension.
This new data structure has lead to performance improvements in our whole code base.
The largest improvement is in accessing subsets of Atoms which is now over 40 times faster (Table :ref:`tab:performance-slicing-atomgroup`), an operation that is used everywhere in MDAnalysis.
Speed-ups of a factor of around five to seven were realized for accessing Atom attributes for whole AtomGroup instances (Table :ref:`tab:performance-accessing-attributes`).
The improved topology data structures are also much faster to initialize, which translates into speed-ups of about three for the task of loading a system from a file (for instance, in the Gromacs GRO format or the Protein Databank PDB format) into a `Universe` instance (Table :ref:`tab:performance-loading-gro`).
Given that for systems with 10 M atoms this process used to take over 100 s, the reduction in load time down to a third is a substantial improvement — and it came essentially "for free" as a by-product of improving the underlying topology data structures.


.. table:: Performance comparison of subselecting an AtomGroup from an existing one using the  new system (upcoming release v0.16.0) against the old (v0.15.0). Subselections were slices of the same size (82,056 atoms). Shorter processing times are better. The benchmarks systems were taken from the `vesicle library`_ :cite:`Kenney:2015aa` and are listed with their approximate number of particles ("# atoms"). Benchmarks were performed on a laptop with an Intel Core i5 2540M 2.6 GHz processor, 8 GB of RAM and a SSD drive. :label:`tab:performance-slicing-atomgroup`

      +----------+----------+----------+----------+
      | # atoms  | v0.15.0  | v0.16.0  | speed up |
      +==========+==========+==========+==========+
      | 1.75 M   |    19 ms | 0.45 ms  |  42      |
      +----------+----------+----------+----------+
      | 3.50 M   |    18 ms | 0.54 ms  |  33      |
      +----------+----------+----------+----------+
      | 10.1 M   |    17 ms | 0.45 ms  |  38      |
      +----------+----------+----------+----------+

.. table:: Performance comparison of accessing attributes with new AtomGroup data structures (upcoming release v0.16.0) compared with the old Atom classes (v0.15.0). Shorter access times are better. The same benchmark systems as in Table :ref:`tab:performance-slicing-atomgroup` were used. :label:`tab:performance-accessing-attributes`

      +----------+----------+----------+----------+
      | # atoms  | v0.15.0  | v0.16.0  | speed up |
      +==========+==========+==========+==========+
      | 1.75 M   | 250 ms   | 35 ms    |   7.1    |
      +----------+----------+----------+----------+
      | 3.50 M   | 490 ms   | 72 ms    |   6.8    |
      +----------+----------+----------+----------+
      | 10.1 M   | 1500 ms  | 300 ms   |   5.0    |
      +----------+----------+----------+----------+

.. table:: Performance comparison of loading a topology file with 1.75 to 10 million atoms with new AtomGroup data structures (upcoming release v0.16.0) compared with the old Atom classes (v0.15.0). Shorter loading times are better. The same benchmark systems as in Table :ref:`tab:performance-slicing-atomgroup` were used. :label:`tab:performance-loading-gro`

      +----------+-----------+----------+----------+
      | # atoms  | v0.15.0   | v0.16.0  | speed up |
      +==========+===========+==========+==========+
      | 1.75 M   | 18 s      | 5 s      |  3.6     |
      +----------+-----------+----------+----------+
      | 3.50 M   | 36 s      | 11 s     |  3.3     |
      +----------+-----------+----------+----------+
      | 10.1 M   | 105 s     | 31 s     |  3.4     |
      +----------+-----------+----------+----------+

.. _`vesicle library`: https://github.com/Becksteinlab/vesicle_library


.. _`Analysis of large systems`:

Analysis of large systems
-------------------------

MDAnalysis has been used extensively to study extremely large simulation systems for long simulation times.
Marrink and co-workers :cite:`Ingolfsson2014` used MDAnalysis to analyze a realistic model of the  membrane of a mammalian cell with 63 different lipid species and over half a million particles for 40 µs. They discovered that transient domains with liquid-ordered character formed and disappeared on the microsecond time scale, with different lipid species clustering in a lipid-specific manner.
A coarse-grained model of the influenza A virion outer lipid envelope (5 M particles) was simulated for 5 microseconds and the resulting trajectory was analyzed using MDAnalysis :cite:`pmid25703376` and the open source MDAnalysis-based `lipid diffusion analysis code`_,  which calculates the diffusion constants of lipids for spherical structures and planar bilayers :cite:`Reddy:2014aa`.
The construction of the CG dengue virion envelope (1 M particles) was largely dependent on MDAnalysis :cite:`pmid26833387`.
The symmetry operators in the deposited dengue protein shell PDB file were applied to a simulated asymmetric unit in a bilayer, effectively tiling both proteins and lipids into the appropriate positions on the virion surface.

.. figure:: figs/flu_simulations.pdf

   Simulation of a coarse-grained model of the influenza A virion membrane (purple/red) close to a model of the human plasma membrane (brown). **A**: Left: initial frame. Right: system after 40 ns . A horizontal black guide line is used to emphasize the rising plasma membrane position. The images were produced with VMD :cite:`Humphrey:1996aa`. **B**   Maximum :math:`Z` (vertical) coordinate values for the influenza A virus envelope and the plasma membrane are tracked over the course of the simulation, indicating that the membrane rises to rapidly.  :label:`fig:virion`

More recently, a 12.7 M CG particle system combining the influenza A envelope and a model of a plasma membrane :cite:`doi:10.1021/jacs.5b08048` were simulated together (Figure :ref:`fig:virion` A).
MDAnalysis was used to assess the stability of this enormous system by tracking, for example, the changes in :math:`Z` coordinate values for different system components (Figure :ref:`fig:virion` B).
In this case, the membrane appeared to rise too rapidly over the course of 50 ns, which suggests that the simulation system will likely have to be redesigned.
Such large systems are challenging to work with, including their visualization, and analysis of quantities based on particle coordinates is essential to assess the correct behavior of the simulations.

.. _lipid diffusion analysis code:
   https://github.com/tylerjereddy/diffusion_analysis_MD_simulations



Other packages that use MDAnalysis
----------------------------------

The user interface and modular design work well in complex scripted work flows and for interactive work, as discussed in section `Interactive Use and Visualization`_.
MDAnalysis also serves as foundation for other packages.
For example, ProtoMD_ :cite:`Somogyi:2016aa`  is a toolkit that facilitates the development of algorithms for multiscale (MD) simulations and uses MDAnalysis for on-the-fly calculations of the collective variables that drive the coarse-grained degrees of freedom.
The ENCORE_ package :cite:`Tiberti:2015fk` enables users to compare conformational ensembles generated either from simulations alone or synergistically with experiments.
MDAnalysis is also the back end for `ST-analyzer`_ :cite:`Jeong:2014nx`, a standalone graphical user interface tool set to perform various trajectory analyses.
MDSynthesis_ :cite:`Dotson:2016aa` (which is based on  datreant_ (Dotson et al, this issue)) gives a Pythonic interface to molecular dynamics trajectories using MDAnalysis, giving the ability to work with the data from many simulations scattered throughout the file system with ease. It makes it possible to write analysis code that can work across many varieties of simulation, but even more importantly, MDSynthesis allows interactive work with the results from hundreds of simulations at once without much effort.



Conclusions
-----------

MDAnalysis provides a uniform interface to simulation data, which comes in a bewildering array of formats.
It enables users to rapidly write code that is portable and immediately usable in virtually all biomolecular simulation communities.
It has an active international developer community with researchers that are expert developers and users of a wide range of simulation codes.
MDAnalysis is widely used (the original paper :cite:`Michaud-Agrawal:2011fu` has been cited more than 195 times) and forms the foundation for more specialized biomolecular simulation tools.
Ongoing and future developments will improve performance further, introduce transparent parallelization schemes to utilize multi-core and GPU systems efficiently, and interface with the `SPIDAL library`_ for high performance data analytics algorithms :cite:`Qiu:2014aa`.


Acknowledgments
---------------

We thank the members of the MDAnalysis community for their contributions in the form of code contributions (see the file AUTHORS_ in the source distribution for the names of all 44 contributors), bug reports, and enhancement requests.
RG was supported by BBSRC grant BB/J014478/1.
ML was supported by the Max Planck Society.
JB was supported by the TOP programme of Prof. Marrink, financed by the Netherlands Organisation for Scientific Research (NWO).
TR was supported by the Canadian Institutes of Health Research, the Wellcome Trust, the Leverhulme Trust, and Somerville College; computational resources for TR's work were provided by PRACE, HPC-Europa2, CINES (France), and the SBCB unit (Oxford).
MNM was supported by the NWO VENI grant 722.013.010.
SLS was supported in part by a Wally Stoelzel Fellowship from the Department of Physics at Arizona State University.
JD was in part supported by a Wellcome Trust grant 092970/Z/10/Z.
DLD was in part supported by a Molecular Imaging Fellowship from the Department of Physics at Arizona State University.
IMK was supported by a REU supplement to grant ACI-1443054 from the National Science Foundation.
OB was supported in part by grant ACI-1443054 from the National Science Foundation; computational resources for OB's work were in part provided by the Extreme Science and Engineering Discovery Environment (XSEDE), which is supported by National Science Foundation grant number ACI-1053575 (allocation MCB130177 to OB).
The MDAnalysis *Atom* logo was designed by Christian Beckstein.

.. _AUTHORS: https://raw.githubusercontent.com/MDAnalysis/mdanalysis/develop/package/AUTHORS

References
----------
.. We use a bibtex file ``mdanalysis.bib`` and use
.. :cite:`Michaud-Agrawal:2011fu` for citations; do not use manual
.. citations

.. _`SPIDAL library`: http://spidal.org
