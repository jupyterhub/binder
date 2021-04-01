.. howto/delete_deployment::

=============================================
Delete your deployed Binder repository
=============================================


Mybinder.org being a free public service without authentication
only has access to things which are already public and in general are not supposed to be deleted.

If you *accidentally* push something that shouldn't be there, you'll need to *ask us* to take it down.

If you remove the commit from your repo, however, outside users
won't be able to get at it on Binder as well.
Binder resolves references with the storage provider (e.g. GitHub) *before* building or launching the image.
If a commit gets pulled from the repo, Binder won't launch it even if a build of that commit is in the cache.

Notice that information might be available to us, the Binder operators, until the cache gets cleared.
So, you are in total control of whether your information is available to the public.
