### Transparency via a Machine-readable Server Identity and Purpose Descriptor.
Mike O'Neill, Febuary 2019

©2019, Baycloud Systems Ltd. All rights reserved.

Web applications store data locally on a user’s computer in order to provide functionality while the user is offline, and to increase performance when the user is online. These local caches have significant advantages for both users and developers, but present risks as well.

A user’s data is both sensitive and valuable; web developers ought to take reasonable steps to protect it. One such step would be to encrypt data before storing it. Another would be to remove data from the user’s machine when it is no longer necessary (for example, when the user signs out of the application, or deletes their account).

Site authors can remove data from a number of storage mechanisms via JavaScript, but others are difficult to deal with reliably. Consider cookies, for instance, which can be partially cleared via JavaScript access to document.cookie. HttpOnly cookies, however, can only be removed via a number of Set-Cookie headers in an HTTP response. This, of course, requires exhaustive knowledge of all the cookies set for a host, which can be complicated to ascertain. Cache is still harder; no imperative interface to a browser’s network cache exists, period.

This document defines a new mechanism to deal with removing data from these and other types of local storage, giving web developers the ability to clear out a user’s local cache of data via the Clear-Site-Data HTTP response header.
