<!-- .slide: class="center" -->
# Building Secure and Resilient APIs

===

## Who am I

<div class="twocolumn">
    <div>
        <ul>
            <li>Proud father</li>
            <li>(Functional) Programming evangelist</li>
            <li>Lead Engineer @ ING API Platform</li>
            <li>Functional and Reactive programming teacher</li>
        </ul>
    </div>
    <div style="text-align:center">
        <img src="images/alessandro.jpg" style="border-radius: 50%; width:50%">
        <img src="images/v3/girl.svg" style="width:50%">
    </div>
</div>

^^^

### Contributions

* Disclaimer
* All nice pictures are by [Beryl Ho](link here todo)
* ?

===
<!-- .slide: class="center" -->

## Introduction



^^^

### Context

* Size of ING group and the amount of developers
    * X countries
    * Y development teams
    * Z different products
    * Much History
* Morph IT landscape into homogeneous landscape
* Started in 2016 (?)
* This talk is about the SDK and API Suite (todo: clarify)

^^^

### Principles

*   Everything should be self-sufficient => Signed AT and PT
    *   Touch on Permissions api that we cannot do that everywhere
*   Consuming APIs should be as convenient for Internal Developer as for an external one.
    * Finding instances, load-balancing, up-time, etc.

^^^

### Technical Principles

*   Global Addressable & Routable name/ip space
*   Zero Trust Networking => mTLS everywhere
*   Set of modular, cherry-pickable components and an off-the-shelf framework providing SDK for Global, 
    *   Ofts framework Spring-Boot
    *   Stick with framework idiomatic ways
    *   Note: Makes service loading hard to use

Note:
* BeyondCorp

^^^

### Mission Statement

> Enable teams to deliver business value with APIs;<br>
> &nbsp; – Secure, Resilient, Minimal Effort

===

## Terminology 
#### "The Meta-Model"

^^^

* *API*<br>
    * Specification of endpoints<br><!-- .element: class="fragment" -->
    * Swagger <!-- .element: class="fragment" -->
* <!-- .element: class="fragment" --> *Endpoint*<br>
    * <!-- .element: class="fragment" --> A single host, method, and pathTemplate
* <!-- .element: class="fragment" --> *Service*<br>
    * <!-- .element: class="fragment" --> A named and owned entity
* <!-- .element: class="fragment" -->*Service Version*<br>
    * <!-- .element: class="fragment" --> Specific version of a service implementing one or more endpoints
* <!-- .element: class="fragment" --> *Instance*<br>
    * <!-- .element: class="fragment" --> Process hosting a specific Service Version


^^^

### Further Material

[API Versioning for Zero Downtime](https://www.youtube.com/watch?v=RvcDs_JLc0Y) – Patrice Krakow ([@patricekrakow](https://twitter.com/patricekrakow)).


===

## Developer Landscape

^^^

### Principles

* Globally Addressable and Routable namespace
* Internal Dev == External Dev (todo: clear in wording)
* Zero Trust Networking (mTLS everywhere)
* REST/HTTP with JSON

Note:
- Could be interpreted as internal applications use LB as well...


^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-apix.svg" -->


^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-servicea.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-serviceb.svg" -->

^^^

<!-- .slide: data-background="images/v1/Labyrinth_03.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-endpointdiscovery.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-register.svg" -->

^^^

<!-- .slide: data-background="images/v1/Labyrinth_01.svg" -->

^^^

<!-- .slide: data-background="images/v1/Labyrinth_02.svg" -->


^^^


<!-- .slide: data-background="images/network-landscape/network-landscape-registerwithmanifest.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab-mtls.svg" -->

^^^

<!-- .slide: data-background="images/v2/menu-and-receipt_02.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab-with-peertoken.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-apigateway.svg" -->

Note:

* Remark on difficulties here?


^^^

### Authentication

<!-- .slide: data-background="images/v2/ID_01.svg" -->

Note:
* Multiple means, username/pw, mobile, etc.


^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-accesstoken.svg" -->

^^^

### Permissions

* Thousands of business rules
* Open vs Closed questions
* Very hard to distribute per API
* Central API; Distributed Deployment

Note:

* Hard to implement per api because 

^^^

### Authentication

<!-- .slide: data-background="images/network-landscape/network-landscape-accesstoken.svg" -->


^^^
### Permissions API

<!-- .slide: data-background="images/network-landscape/network-landscape-permissionsapi.svg" -->

^^^

### Confidence Checking

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-deployment.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-request.svg" -->

^^^

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-success.svg" -->


^^^

### Monitoring

<!-- .slide: data-background="images/network-landscape/network-landscape-monitoring.svg" -->


===
<!-- .slide: class="center" -->
## Enough with the pictures already

Note:

* Finagle is already quite resilient by default but can be more performant


^^^

### Use Response Classifiers

TODO

^^^

### Java-Friendly Response Classifiers

TODO

^^^

### Stack Modifications
<!-- .slide: data-background="images/clientstack-apisdk.svg" -->

Note:

* Automatic Addition of Peer Tokens
* Tracing integration with OpenTracing
* Finagle uses the Response Classifiers for metrics and unavailability, but not for retry.
* Debug module for printing exactly what goes over the wire, useful when using TLS, doesn't require mITM.

^^^

### Service Discovery integration

```scala
Http.client.newService("phoenix!api.ing.com:GET:/services/{name}")
```

---

* PhoenixResolver
* TODO

^^^

### DeferringResolver

* Java's Service Loading is not idiomatic to all frameworks
    * e.g. Spring Boot
* Defer loading of the PhoenixResolver until when it can be configured
  framework idiomatically and set
* Until the actual resolver is set all `Var`s are started with `Pending`.

^^^

### Service Name Verifier
* TODO

^^^

### Confidence Checking / DTAB
* TODO do not allow just any dtab

^^^


===
<!-- .slide: class="center" -->
# Lessons Learned

^^^

### Education, Education, Education

^^^

### Setting Timeouts

```scala
.withRequestTimeout(...)
.configured(TimeoutFactory.Param(...)) // Only effective when using MethodBuilder
.withTransport.connectTimeout(...)
```

#### Total timeouts
```scala
new TimeoutFilter(..., ...)
```

#### Fine grained timeouts

```scala
.withSession().acquisitionTimeout(...)
.withTransport.readTimeout(...)
.withTransport.writeTimeout(...)
```

^^^

### Beware of downing instances

* Functional Errors may occur for a small number of customers only
* Downing instances can lead to unavailability for other customers as well


^^^

### Load-Balancers

Note:

* Be wary when other load-balancers enter the stack. Unexpected behaviour might occur. Has effects on retries and failure accrual.
* 

^^^

### Stick to REST

^^^

### Keep everyone up-to-date

Note:
* Fixes bugs
* Improves performance
* Enables you to activate H2 / new features

===
<!-- .slide: class="center" -->
# Future Work

^^^
## Future work
*   Store SLA in PeerToken
*   Support concurrent Live API Specs
*   Move parts to Kubernetes
*   Investigate [Tunables](https://twitter.github.io/finagle/guide/Configuration.html#tunables)
*   Idempotent POSTs with UUIDs and framework-provided 'transactions' and use nack headers
*   HTTP2   
*   Investigate gRPC

===

???

===
<!-- .slide: class="center" -->
# Concluding

^^^

## Summary

* Architecture
* Education, Education, Education
* Monitoring

^^^

<!-- .slide: class="center" -->
# Thank You

<small>Yes, we are hiring :)</small>
