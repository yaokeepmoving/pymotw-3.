====================================
 Counter --- count hashable objects
====================================

A :class:`Counter` is a container that keeps track of how many times
equivalent values are added.  It can be used to implement the same
algorithms for which other languages commonly use bag or multiset data
structures.

Initializing
============

:class:`Counter` supports three forms of initialization.  Its
constructor can be called with a sequence of items, a dictionary
containing keys and counts, or using keyword arguments mapping string
names to counts.

.. literalinclude:: collections_counter_init.py
   :caption:
   :start-after: #end_pymotw_header

The results of all three forms of initialization are the same.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_init.py'))
.. }}}

.. code-block:: none

	$ python3 collections_counter_init.py
	
	Counter({'b': 3, 'a': 2, 'c': 1})
	Counter({'b': 3, 'a': 2, 'c': 1})
	Counter({'b': 3, 'a': 2, 'c': 1})

.. {{{end}}}

An empty :class:`Counter` can be constructed with no arguments and
populated via the :func:`update` method.

.. literalinclude:: collections_counter_update.py
   :caption:
   :start-after: #end_pymotw_header

The count values are increased based on the new data, rather than
replaced.  In this example, the count for ``a`` goes from ``3`` to
``4``.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_update.py'))
.. }}}

.. code-block:: none

	$ python3 collections_counter_update.py
	
	Initial : Counter()
	Sequence: Counter({'a': 3, 'b': 2, 'c': 1, 'd': 1})
	Dict    : Counter({'d': 6, 'a': 4, 'b': 2, 'c': 1})

.. {{{end}}}

Accessing Counts
================

Once a :class:`Counter` is populated, its values can be retrieved
using the dictionary API.

.. literalinclude:: collections_counter_get_values.py
   :caption:
   :start-after: #end_pymotw_header

:class:`Counter` does not raise :class:`KeyError` for unknown items.
If a value has not been seen in the input (as with ``e`` in this
example), its count is ``0``.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_get_values.py'))
.. }}}

.. code-block:: none

	$ python3 collections_counter_get_values.py
	
	a : 3
	b : 2
	c : 1
	d : 1
	e : 0

.. {{{end}}}

The :func:`elements` method returns an iterator that produces all of
the items known to the :class:`Counter`.

.. literalinclude:: collections_counter_elements.py
   :caption:
   :start-after: #end_pymotw_header

The order of elements is not guaranteed, and items with counts less
than or equal to zero are not included.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_elements.py', break_lines_at=65))
.. }}}

.. code-block:: none

	$ python3 collections_counter_elements.py
	
	Counter({'e': 3, 'x': 1, 'm': 1, 't': 1, 'y': 1, 'l': 1, 'r': 1, 
	'z': 0})
	['x', 'm', 't', 'e', 'e', 'e', 'y', 'l', 'r']

.. {{{end}}}

Use :func:`most_common` to produce a sequence of the *n* most
frequently encountered input values and their respective counts.

.. literalinclude:: collections_counter_most_common.py
   :caption:
   :start-after: #end_pymotw_header

This example counts the letters appearing in all of the words in the
system dictionary to produce a frequency distribution, then prints the
three most common letters.  Leaving out the argument to
:func:`most_common` produces a list of all the items, in order of
frequency.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_most_common.py'))
.. }}}

.. code-block:: none

	$ python3 collections_counter_most_common.py
	
	Most common:
	e:  235331
	i:  201032
	a:  199554

.. {{{end}}}

Arithmetic
==========

:class:`Counter` instances support arithmetic and set operations for
aggregating results. This example shows the standard operators for
creating new :class:`Counter` instances, but the in-place operators
``+=``, ``-=``, ``&=`` and ``|=`` are also supported.

.. literalinclude:: collections_counter_arithmetic.py
   :caption:
   :start-after: #end_pymotw_header

Each time a new :class:`Counter` is produced through an operation, any
items with zero or negative counts are discarded.  The count for ``a``
is the same in :data:`c1` and :data:`c2`, so subtraction leaves it at
zero.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_counter_arithmetic.py',
..                    break_lines_at=74, line_break_mode='wrap'))
.. }}}

.. code-block:: none

	$ python3 collections_counter_arithmetic.py
	
	C1: Counter({'b': 3, 'a': 2, 'c': 1})
	C2: Counter({'a': 2, 'b': 1, 'p': 1, 't': 1, 'l': 1, 'e': 1, 'h': 1})
	
	Combined counts:
	Counter({'b': 4, 'a': 4, 'p': 1, 't': 1, 'c': 1, 'e': 1, 'l': 1, 'h': 1})
	
	Subtraction:
	Counter({'b': 2, 'c': 1})
	
	Intersection (taking positive minimums):
	Counter({'a': 2, 'b': 1})
	
	Union (taking maximums):
	Counter({'b': 3, 'a': 2, 'p': 1, 't': 1, 'c': 1, 'e': 1, 'l': 1, 'h': 1})

.. {{{end}}}
