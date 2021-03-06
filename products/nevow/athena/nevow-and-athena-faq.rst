Nevow & Athena FAQ
==================


Q) When I reload a page, the server logs a traceback ending with
   ``exceptions.AssertionError: Cannot render a LivePage more than once``

A) LivePage instances maintain server-side state that corresponds to the
   connection to the browser.  Because of this, each LivePage instance can only
   be used to serve a single client.  When you serve LivePages, make sure that
   you create a new instance for each render pass.

Q) I can't debug athena on Internet explorer.  what gives?

A) Athena include the livefragment javascript in the body of the document via a
   script tag.  Visual studio doesn't appear to be happy with this and does not
   let you set break points.  If you load the javascript in the head of the
   document then you can set a break point in your athena livefragment
   javascript using visual studio.  The best way I found to do this is manually
   include the javacript in the header and then information athena that the
   livefragment javascript is already loaded.  One way to do this is to call the
   'hidden' _shouldInclude method on the athena livepage instance, e.g
   self._shouldInclude('yourjsmodule').  This will let athena know that the
   javascript is already loaded and not to load it twice.

Q) Why doesn't Athena support Safari?

A) Safari has a broken JS implementation that throws an error when a nested
   'named' function is encountered, e.g.
   
   
   ::
   
       methods(function foo(self) {},function bar(self) {});
   

   A workaround would be to use anonymous functions instead of named functions.
   Another issue is that someone needs to implement a custom runtime module in
   runtime.js to support Safari.

Q) How can I unit-test javascript code using Athena?

A) The same way you unit-test your ordinary `Twisted` or `Divmod` software: by
   using `trial`. You can find :doc:`athena-testing`. Have a look at
   `nevow.test.test_javascript` to see, how tests are prepared and run.
