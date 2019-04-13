# Transparency via a Machine-readable Server Identity and Purpose Descriptor.
Mike O'Neill, Febuary 2019

©2019, Baycloud Systems Ltd. All rights reserved.

## Introduction
Web applications can ensure that all data associated with a controlled origin is deleted by using the Clear Site Data API. 
By including a `Clear-Site-Data` header in a response all or particular categories of stored data can be immediately removed, including cookies, local storage, indexed database storage, javascript execution contexts, the client-side cache etc.

This document describes a mechanism whereby the data can be automatically deleted after a specified period, without requiring a further request to the controlling origin.

This would allow applications to store data, such as cookies or local storage, for short periods to support required functionality such as  "session state", but ensure this data is not retained after a defined period. 

The cookie API already contain such a mechanism, i.e. the `expires` or `max-age` attribute, this API extends a similar mchanism to all origin controlled storage. 

It is envisioned that user agent suppliers will specify reasonable defaults for the duration of stored data where servers do not include the response header, as part of their priacy and personal data protecting function.

Site authors can remove data from a number of storage mechanisms via JavaScript, but others are difficult to deal with reliably. Data stored as objects in `IndexdedDB` transactional databases are hard to detect outside of the scripts that use them. HttpOnly cookies are inaccessible to script, only removable via Set-Cookie headers in HTTP responses. This document defines an HTTP based mechanism to specify durations for all storage, and a JavaScript API so script libraries can offer the same functionality.

## Retain-Storage header
Sites can ensure that all storage is removed after a defined period by returning the `Retain-Storage` response header.

`Retain-Storage: max-age=3600 ,"cache", "cookies", "storage", "executionContexts"`

The categories of storage are the same as defined for Clear-Site-Data.

User agents would then ensure that all the specified storage is deleted when the number of seconds indicated by the `Max-Age` attribute after the time when the `Retain-Storage` header was received. Subsequent ``Retain-Storage` headers could only result in lowering the associated duration i.e. it cannot be increased after it has been set.

After the duration has been exceeded the relevant storage MUST be deleted at least before a request is sent to the associted origin, or any associated browsing context starts is initialised.

## Javascript API
Script can execute the following JavaScript function to determine the durations associated with each storage cattegory for the script origin.

```
navigator.retainStorage.Get()

## Prior Art
*  The Clear-Site-Data API defines a mechanism to remove data from local storage, giving web developers the ability to clear out a user’s local cache of data via the Clear-Site-Data HTTP response header. "[Clear Site Data](https://www.w3.org/TR/clear-site-data/)"


*   Mike West has proposed a mechanism which allows HTTP servers to maintain stateful sessions with HTTP user agents, addressing some of the security and privacy considerations of HTTP Cookies. "[HTTP State Tokens](https://mikewest.github.io/http-state-tokens/draft-west-http-state-tokens.html)" 

*   The Tracking Protection Working Group discussed limits on all browser storage (identifiers) to a default short duration when DNT:1 was set e.g. "[public-tracking@w3.org Mail Archives](https://lists.w3.org/Archives/Public/public-tracking/2013Jun/0262.html)" 

* Safari's ITP 2.1 limits the duration of first-party cookies created by script to a default 7 days duration. "[Intelligent Tracking Prevention 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/)"



  

