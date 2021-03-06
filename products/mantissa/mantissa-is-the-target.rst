=================================
Mantissa is the Deployment Target
=================================

*Mantissa is the deployment target, not the Web.*

What Does That Mean?
====================

Mantissa is an application server; it can serve an application over several
protocols. HTTP is one of those protocols, but it [HttpIsntSpecial isn't the
only one].

The web interface that Mantissa provides should be pluggable, but it should
''not'' be so flexible that every Mantissa application provides its own fixed
skin and look and feel.

In other words, when you write a Mantissa application, Mantissa is the
deployment target. You deploy your application into a Mantissa application
container that faces the web, '''not''' directly onto the web. Ideally the
Mantissa web container will be able to show components from several different
independently-developed applications simultaneously.

This means that you SHOULD NOT be writing your own IResource implementations
or full-page templates; applications should be plugging in somewhere on the
page. The set of interfaces developed for this right now (INavigableFragment,
INavigableElement) are not necessarily the best, but others will arise soon.

This is not gold-plating or over-engineering, as many seem to think; it
springs from a specific requirement. Divmod specifically intends to develop
something like 10 different applications, and launch each one separately, but
make it easy for people to sign up and for our subscribers to activate new
services. Each of those applications is pluggable and has integration points
with other applications. These applications are all going to share the same
web interface and should have a common look and feel, common interface
elements, and a shared infrastructure (for example: password management,
search).

Examples of Where This Matters
==============================

The most trivial example is side-by-side viewing.  It would be good if the
tab-based navigation system could move towards being one where you click on a
tab to 'go to a page', vs. one where you click on a tab to 'launch' a Mantissa
object, enabling side-by-side viewing of multiple interactive objects on one
LivePage. (Donovan's ill-fated launch-bar demonstration is what I mean by
this, for those who saw it before it was deleted.)

Obviously you cannot lay out the entire page in one component if it is going
to be displayed on the same page as a separate component.

A better example might be search. There should be one search box for every
Mantissa application. When a user searches, each application should be queried
(where 'application' in this case is equivalent to 'thing installed on a
user's store, implementing the appropriate search interface')

The search results page should not be laid out or rendered by any particular
application, but instead, be an aggregation of all the search results for a
particular term. This means that the search page has to be part of the
framework, not part of an application.
