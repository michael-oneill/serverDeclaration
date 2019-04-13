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

## Prior Art
*   Mike West has proposed a way for origins to assert they belong to a set managed by the same top-level or "first party" resource "[First-Party Sets](https://github.com/mikewest/first-party-sets)" 

*   The Tracking Protection Working Group's "[Tracking Preference Expression (DNT)](https://www.w3.org/TR/tracking-dnt/)" defined a server transparency declaration at `/.well-known/dnt/`
    This was designed to allow the entity managing any server (first-party or subresource) to declare various properties to aid transparency.

*   John Wilander has proposed amendments to the Same Origin Policy so sets of domains could be trusted as if they were first-party. "[Single Trust and Same-Origin Policy v2](https://lists.w3.org/Archives/Public/public-webappsec/2017Mar/0034.html)"

*   There is ongoing discussion within the IETF about recognising domain name "relatedness" in DNS records "[Related Domains By DNS](https://datatracker.ietf.org/doc/draft-brotman-rdbd/)"

*   Cookie Name Prefixes are discussed in the under-development replacement for RFC 6265 "[Cookies: HTTP State Management Mechanism draft-ietf-httpbis-rfc6265bis-02](https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-02#page-14)"

*   The IAB EU's "[IAB Europe Transparency & Consent Framework](https://advertisingconsent.eu/)" defines an externally hosted JSON resource that identifies Advertising technology vendors and a set of defined purposes.

*   There is ongoing work defining an origin wide server Origin Policy Manifest file at a well-known location. "[Origin Policy](https://wicg.github.io/origin-policy/)".
  

