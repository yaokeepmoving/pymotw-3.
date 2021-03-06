.. _collections-namedtuple:

=================================================
 namedtuple --- tuple subclass with named fields
=================================================

The standard :class:`tuple` uses numerical indexes to access its
members.

.. literalinclude:: collections_tuple.py
   :caption:
   :start-after: #end_pymotw_header

This makes :class:`tuples` convenient containers for simple uses.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_tuple.py'))
.. }}}

.. code-block:: none

	$ python3 collections_tuple.py
	
	Representation: ('Bob', 30, 'male')
	
	Field by index: Jane
	
	Fields by index:
	Bob is a 30 year old male
	Jane is a 29 year old female

.. {{{end}}}

On the other hand, remembering which index should be used for each
value can lead to errors, especially if the :class:`tuple` has a lot
of fields and is constructed far from where it is used.  A
:class:`namedtuple` assigns names, as well as the numerical index, to
each member.

Defining
========

:class:`namedtuple` instances are just as memory efficient as regular
tuples because they do not have per-instance dictionaries.  Each kind
of :class:`namedtuple` is represented by its own class, created by
using the :func:`namedtuple` factory function.  The arguments are the
name of the new class and a string containing the names of the
elements.

.. literalinclude:: collections_namedtuple_person.py
    :caption:
    :start-after: #end_pymotw_header

As the example illustrates, it is possible to access the fields of the
:class:`namedtuple` by name using dotted notation (``obj.attr``) as
well as using the positional indexes of standard tuples.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_person.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_person.py
	
	
	Representation: Person(name='Bob', age=30)
	
	Field by name: Jane
	
	Fields by index:
	Bob is 30 years old
	Jane is 29 years old

.. {{{end}}}

Just as with a regular :class:`tuple`, a :class:`namedtuple` is
immutable. This restriction allows :class:`tuple` instances to have a
consistent hash value, which makes it possible to use them as keys in
dictionaries and to be included in sets.

.. literalinclude:: collections_namedtuple_immutable.py
   :caption:
   :start-after: #end_pymotw_header

Trying to change a value through its named attribute results in an
:class:`AttributeError`.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_immutable.py', ignore_error=True))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_immutable.py
	
	
	Representation: Person(name='Pat', age=12)
	Traceback (most recent call last):
	  File "collections_namedtuple_immutable.py", line 17, in <modul
	e>
	    pat.age = 21
	AttributeError: can't set attribute

.. {{{end}}}

Invalid Field Names
===================

Field names are invalid if they are repeated or conflict with Python
keywords.

.. literalinclude:: collections_namedtuple_bad_fields.py
   :caption:
   :start-after: #end_pymotw_header

As the field names are parsed, invalid values cause
:class:`ValueError` exceptions.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_bad_fields.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_bad_fields.py
	
	Type names and field names cannot be a keyword: 'class'
	Encountered duplicate field name: 'age'

.. {{{end}}}

In situations where a :class:`namedtuple` is being created based on
values outside of the control of the program (such as to represent
the rows returned by a database query, where the schema is not known
in advance), set the *rename* option to ``True`` so the invalid fields
are renamed.

.. literalinclude:: collections_namedtuple_rename.py
   :caption:
   :start-after: #end_pymotw_header

The new names for renamed fields depend on their index in the tuple,
so the field with name ``class`` becomes ``_1`` and the duplicate
``age`` field is changed to ``_2``.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_rename.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_rename.py
	
	('name', '_1', 'age')
	('name', 'age', '_2')

.. {{{end}}}

Special Attributes
==================

:class:`namedtuple` provides several useful attributes and methods for
working with subclasses and instances. All of these built-in
properties have names prefixed with underscore (``_``), which by
convention in most Python programs indicates a private
attribute. However, for :class:`namedtuple` the prefix is intended to
protect the name from collision with user-provided attribute names.

The names of the fields passed to :class:`namedtuple` to define the
new class are saved in the :attr:`_fields` attribute.

.. literalinclude:: collections_namedtuple_fields.py
   :caption:
   :start-after: #end_pymotw_header

Although the argument is a single space-separated string, the stored
value is the sequence of individual names.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_fields.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_fields.py
	
	
	Representation: Person(name='Bob', age=30)
	Fields: ('name', 'age')

.. {{{end}}}

:class:`namedtuple` instances can be converted to :class:`OrderedDict`
instances using :func:`_asdict`.

.. literalinclude:: collections_namedtuple_asdict.py
   :caption:
   :start-after: #end_pymotw_header

The keys of the :class:`OrderedDict` are in the same order as the
fields for the :class:`namedtuple`.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_asdict.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_asdict.py
	
	
	Representation: Person(name='Bob', age=30)
	As Dictionary: OrderedDict([('name', 'Bob'), ('age', 30)])

.. {{{end}}}

The :func:`_replace` method builds a new instance, replacing the
values of some fields in the process.

.. literalinclude:: collections_namedtuple_replace.py
   :caption:
   :start-after: #end_pymotw_header

Although the name implies it is modifying the existing object, because
:class:`namedtuple` instances are immutable the method actually
returns a new object.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'collections_namedtuple_replace.py'))
.. }}}

.. code-block:: none

	$ python3 collections_namedtuple_replace.py
	
	
	Before: Person(name='Bob', age=30)
	After: Person(name='Robert', age=30)
	Same?: False

.. {{{end}}}

