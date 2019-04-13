# Transparency via a Machine-readable Server Identity and Purpose Descriptor.
Mike O'Neill, Febuary 2019

©2019, Baycloud Systems Ltd. All rights reserved.

## Introduction
Web applications can ensure that all data associated with a controlled origin is deleted by using the Clear Site Data API. 
By including a `Clear-Site-Data` header in a response all or particular categories of stored data can be immediately removed, including cookies, local storage, indexed database storage, javascript execution contexts, the client-side cache etc.

This document describes a procedure whereby the data can be automatically deleted after a specified period, without requiring a further request to the controlling origin.

This would allow applications to store data, such as cookies or local storage, for short periods to support required functionality such as  "session state", but ensure this data is not retained after a defined period. 

The cookie API already contain such a mechanism, i.e. the `expires` or `max-age` attribute, this API extends that to all origin controlled storage. 

It is envisioned that user agent suppliers interested in protecting the privacy and personal data of their users will specify reasonable defaults where servers do not include the response header.



Web applications store data locally on a user’s computer in order to provide functionality while the user is offline, and to increase performance when the user is online. These local caches have significant advantages for both users and developers, but present risks as well.

A user’s data is both sensitive and valuable; web developers ought to take reasonable steps to protect it. One such step would be to encrypt data before storing it. Another would be to remove data from the user’s machine when it is no longer necessary (for example, when the user signs out of the application, or deletes their account).

Site authors can remove data from a number of storage mechanisms via JavaScript, but others are difficult to deal with reliably. Consider cookies, for instance, which can be partially cleared via JavaScript access to document.cookie. HttpOnly cookies, however, can only be removed via a number of Set-Cookie headers in an HTTP response. This, of course, requires exhaustive knowledge of all the cookies set for a host, which can be complicated to ascertain. Cache is still harder; no imperative interface to a browser’s network cache exists, period.

This document defines a new mechanism to deal with removing data from these and other types of local storage, giving web developers the ability to clear out a user’s local cache of data via the Clear-Site-Data HTTP response header.
