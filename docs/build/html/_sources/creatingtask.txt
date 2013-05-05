Running the celery worker server
================================

ou now run the worker by executing our program with the worker argument:

$ celery -A tasks worker --loglevel=info

In production you will want to run the worker in the background as a daemon. To do this you need to use the tools provided by your platform, or something like supervisord (see Running the worker as a daemon for more information).

For a complete listing of the command line options available, do:

$  celery worker --help

There also several other commands available, and help is also available:

$ celery help


