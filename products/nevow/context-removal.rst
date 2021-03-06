===============
Context Removal
===============

Step 1
======

*Change Resource Inheritance*

The ``Page`` class is now imported from ``nevow.page`` instead of
``nevow.rend``. Class declarations change as indicated below.

*From:*

::

    class Root(rend.Page):
    ...

*To:*

::

    class Root(page.Page):
    ...


Step 2
======

*Change ``child_`` Class Attributes*

Class ``child_*`` class attributes change in that the prefix is no longer needed
and all ``child_``\ s are stored together in a ``dict``.

*From:*

::

    ...
      child_foo = RendPageInstance
    ...


*To:*

::

    ...
      children = {
        'foo': PagePageInstance,
      ...}
    ...


Step 3
======

*Remove Context from Signatures*

The ``context`` object is no longer passed in the method signatures. Passed
parameters changes as indicated below.

*From:*

::

    ...
      def render_foo(self, context, data):
      ...



*To:*

::

    ...
      def render_foo(self, request, tag):
      ...


Step 4
======

*Use Decorators*

The ``method`` names no longer need to contain ``render_`` and ``child_``.
Method names change as indicated below.

*From:*

::

    ...
      def render_foo(self, context, data):
      ...
    ...
      def child_bar(self, context):
      ...


*To:*

::

    ...
      def foo(self, request, tag):
      ...
      page.renderer(foo)
    ...
      def bar(self, request):
      ...
      page.child(bar)


Or, for a version of python that supports the decorator syntax:

::

    ...
      @page.renderer
      def foo(self, request, tag):
      ...
    ...
      @page.child
      def bar(self, request):
      ...


Step 5
======

*Change Fill Slot Calls*

The ``fillSlots()`` calls are still ``tag`` methods, but ``tag`` is now passed
directly to ``render`` methods and not accessed as a ``context`` attribute. Make
changes as indicated below.

*From:*

::

    ...
      def render_entries(self, ctx, data):
        ctx.tag.fillSlots('author', 'The Humble Author')
        ctx.tag.fillSlots('title', 'The Excellent Title')
        ctx.tag.fillSlots('content', 'The Interesting Content')
        return ctx.tag


*To:*

::

    ...
      def entries(self, request, tag):
        tag.fillSlots('author', 'The Humble Author')
        tag.fillSlots('title', 'The Excellent Title')
        tag.fillSlots('content', 'The Interesting Content')
        return tag
      page.renderer(entries)
