Calling the task
================

To call our task you can use the delay() method.

This is a handy shortcut to the apply_async() method which gives greater control of the task execution:

>>> from tasks import add
>>> add.delay(4, 4)

The task has now been processed by the worker you started earlier, and you can verify that by looking at the workers console output.

Calling a task returns an AsyncResult instance, which can be used to check the state of the task, wait for the task to finish or get its return value (or if the task failed, the exception and traceback). But this isn’t enabled by default, and you have to configure Celery to use a result backend, which is not discussed as part of this tutorial.

Options
-------

With results backend enabled, celery provides lots of options to play with.

Assuming that the result backend is configured, let’s call the task again. This time you’ll hold on to the AsyncResult instance returned when you call a task:

>>> result = add.delay(4, 4)

The ready() method returns whether the task has finished processing or not:

>>> result.ready()
False

You can wait for the result to complete, but this is rarely used since it turns the asynchronous call into a synchronous one:

>>> result.get(timeout=1)
8

In case the task raised an exception, get() will re-raise the exception, but you can override this by specifying the propagate argument:

>>> result.get(propagate=True)

If the task raised an exception you can also gain access to the original traceback:

>>> result.traceback
...

