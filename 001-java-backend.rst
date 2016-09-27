*********************
NEP-001: Java backend
*********************

=================  ===================================
Stage              Idea
Proposed by        Trevor Bekolay <tbekolay@gmail.com>
Approved by
Implemented by
Completion date
Implemented in
=================  ===================================

Context
=======

Hopefully we're in agreement that moving to Python
was a net positive for Nengo!
Java's still a powerhouse in terms of language use,
and it does multithreading better than most other languages.
However, most importantly, it's the language Android uses.
With a Java backend, we could write Nengo models
with the usual frontend code,
and have the backend spit out some Java class files
that implement that specific model,
which could easily be used in an Android application.

Proposal
========

Create a Java backend for Nengo
by co-opting the existing Nengo 1.4 codebase.

Details
=======

Like all other backends,
the Java backend would be encapsulated
in a ``Simulator`` object::

  with nengo.Network() as net:
      # Model stuff
  with nengo_java.Simulator(net) as sim:
      sim.run(1.0)

The main difficulty with the Java backend
is going to be dealing with the
Java / Python interface.
If we constrain the interaction
to be only one-way,
the interface is easy to define,
but it means that nodes have to be
significantly simplified.

Two-way interaction is possible;
we did it in Nengo 1.4 using `Jython <http://www.jython.org/>`_,
which has progressed a bit over the past few years.
There are also some new projects aiming to do
two-way Java / Python interop,
most notably `PyJNIus <https://github.com/kivy/pyjnius>`_.

The first step in the implementation will be
evaluating these interop methods
and making some quick prototypes.
Once an interop method has been chosen,
the existing Nengo 1.4 simulator code
should be used;
the ``Simulator`` class would essentially be
a wrapper around the core Nengo 1.4 simulator code.

Note that at one point I filtered the Nengo 1.4 history
to include only the simulator files.
It's available as
`tbekolay/nengo_java <https://github.com/tbekolay/nengo_java>`_
for whoever wants to implement this backend.

Pros and cons
=============

Reasons to implement this backend:

* Enables Nengo models running on Android phones.
* Less effort than other backends due to existing Nengo 1.4 codebase.

Reasons not to implement this backend:

* Java / Python interop is difficult, and none of the options are perfect.
  `Yak shaving <http://sethgodin.typepad.com/seths_blog/2005/03/dont_shave_that.html>`_
  is inevitable.
* A Java backend would not allow arbitrary models to run on Android apps
  due to requiring Python to construct the model.
