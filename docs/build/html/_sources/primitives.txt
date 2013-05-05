The Primitives
==============

The primitives are subtasks themselves, so that they can be combined in any number of ways to compose complex workflows.

Note:

These examples retrieve results, so to try them out you need to configure a result backend. The example project above already does that

Group
-----
A group calls a list of tasks in parallel, and it returns a special result instance that lets you inspect the results as a group, and retrieve the return values in order.

>>> from celery import group
>>> from proj.tasks import add

>>> group(add.s(i, i) for i in xrange(10))().get()
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

    Partial group

>>> g = group(add.s(i) for i in xrange(10))
>>> g(10).get()
[10, 11, 12, 13, 14, 15, 16, 17, 18, 19]


Chains
------

Tasks can be linked together so that after one task returns the other is called:

>>> from celery import chain
>>> from proj.tasks import add, mul

# (4 + 4) * 8
>>> chain(add.s(4, 4) | mul.s(8))().get()
64

or a partial chain:

# (? + 4) * 8
>>> g = chain(add.s(4) | mul.s(8))
>>> g(4).get()
64

Chains can also be written like this:

>>> (add.s(4, 4) | mul.s(8))().get()
64


Chord
-----
A chord is a group with a callback:

>>> from celery import chord
>>> from proj.tasks import add, xsum

>>> chord((add.s(i, i) for i in xrange(10)), xsum.s())().get()
90

A group chained to another task will be automatically converted to a chord:

>>> (group(add.s(i, i) for i in xrange(10)) | xsum.s())().get()
90


Be sure to read more about workflows in the `Canvas user guide <http://docs.celeryproject.org/en/latest/userguide/canvas.html#guide-canvas>`_. 

