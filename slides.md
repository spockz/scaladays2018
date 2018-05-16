<!-- .slide: class="center" -->
# Building Secure and Resilient APIs
#### Using Finagle

===

## Who am I

<div class="twocolumn">
    <div>
        <ul>
            <li>Proud father</li>
            <li>(Functional) Programming evangelist</li>
            <li>Functional and Reactive programming teacher</li>
            <li>Global API SDK Lead Engineer @ Global API Platform</li>
        </ul>

        <br>
        <br>
        
        <h3>Contact</h3>
        <ul>
            <li><a href="https://twitter.com/alevermeulen">@alevermeulen</a></li>
            <li><a href="https://www.linkedin.com/in/alessandrovermeulen">LinkedIn</a></li>
        </ul>
    </div>
    <div style="text-align:center">
        <img src="images/alessandro.jpg" style="border-radius: 50%; width:50%">
        <img src="images/v3/girl.svg" style="width:50%">
    </div>
</div>

^^^

## Credits

* @Team API Suite members
* Amazing pictures by [Beryl Ho](https://www.instagram.com/bee.visual/)

===
<!-- .slide: class="center" -->
# Introduction

^^^

## Context

* Size of ING group and the amount of developers
    * 25+ countries
    * 300 development teams
    * Many different products
    * Much History
* Morph IT landscape into homogeneous landscape
* Started in 2016

^^^

## Technical Context

*   Zero Trust Networking
*   Pretty good IT infrastructure
*   APIs build on top of Global API SDK
    * Build in Scala
    * Toolkit: Set of modular, cherry-pickable components 
    * Frameworks:
        * Java: Spring-Boot based
        * Scala: Finch based
    *   Promise to Stick with framework idiomatic ways

*  <!-- .element: class="fragment" -->  Enable teams to deliver business value with APIs;
   Securely, Resilient, with Minimal Effort


Note:
* BeyondCorp
* My team provides this Global API SDK
* IT Infrastructure: things work or they fail hard

^^^

## Overview

* Terminology
* Application Landscape
* Finagle Extensions
* Lessons Learned
* Additional Measures
* Future Work

<p></p>

#### Out of scope

* Naming conventions part of Designing APIs
* Security Scans
* Release strategies

Note:

* Out of scope for this talk
* How to exactly execute strategies such as canary releases


===
<!-- .slide: class="center" -->
# Terminology 

^^^

## The Meta-Model

* *API*<br>
    * <!-- .element: class="fragment" --> Specification of *endpoints*<br>
    * Swagger <!-- .element: class="fragment" -->
* <!-- .element: class="fragment" --> *Endpoint*<br>
    * <!-- .element: class="fragment" --> host (e.g. `api.ing.com`)
    * <!-- .element: class="fragment" --> method (e.g. `GET`|`PUT`|`POST`|`PATCH`|`..`)
    * <!-- .element: class="fragment" --> pathTemplate (e.g. `/accounts/{accountId}`)
* <!-- .element: class="fragment" --> *Service*<br>
    * <!-- .element: class="fragment" --> A named logical component, owned by a team consisting of one to many versions
* <!-- .element: class="fragment" -->*Service Version*<br>
    * <!-- .element: class="fragment" --> Specific version of a *service* implementing one or more *endpoints* from one or more API (specifications)
* <!-- .element: class="fragment" --> *Instance*<br>
    * <!-- .element: class="fragment" --> Running code implementing a specific *Service Version*


^^^

## Further Material

[API Versioning for Zero Downtime](https://www.youtube.com/watch?v=RvcDs_JLc0Y) – Patrice Krakow ([@patricekrakow](https://twitter.com/patricekrakow)).

Note:
* Patrice has dedicated a full talk on the possiblities with our versioning scheme. I will not repeat
  that here.

===
<!-- .slide: class="center" -->
# Application Landscape

^^^

## Principles

* Globally Addressable and Routable namespace
* Internal &asymp; External
* Zero Trust Networking
* Autonomicity
* REST/HTTP with JSON

Note:
- Could be interpreted as internal applications use LB as well...
- The ease of consumption of APIs should be the same internal devs as external
- In other words, if it is not good enough for us it is not good enough for our customers,
  and vice versa.
-  mTLS everywhere

^^^

## Resilience
* Provided by Finagle
    * Performant
    * Load-Balancing
    * Retries
    * Circuit-Breaking

* ["Three Resilience Patterns with Twitter's Finagle"](https://www.youtube.com/watch?v=77KutKGL--E)  - JFall 2017 — Eggie and Effi

^^^
<!-- .slide: class="center" -->
## Intermezzo

^^^

<!-- .slide: data-background="images/v3/Girl.svg" -->

^^^

## Mommy, Candy?
<!-- .slide: data-background="images/v1/Candy_01.svg" -->

^^^

## Daddy, Candy!
<!-- .slide: data-background="images/v1/Candy_02.svg" -->

Note:
* I trust my daughter. Also to be innovative in ways to get more candy

^^^

## Let's see
<!-- .slide: data-background="images/v1/Candy_03.svg" -->

^^^

## Success!
<!-- .slide: data-background="images/v1/Candy_04.svg" -->

Note:
* This is an example of autonomous parts improving resilience

^^^
<!-- .slide: class="center" -->
## </ Intermezzo >


^^^

## In the beginning..

<!-- .slide: data-background="images/network-landscape/network-landscape-apix.svg" -->


^^^

## Grows into a Service

<!-- .slide: data-background="images/network-landscape/network-landscape-servicea.svg" -->

^^^

## Joined by another service

<!-- .slide: data-background="images/network-landscape/network-landscape-serviceb.svg" -->

^^^

<!-- .slide: data-background="images/v1/Labyrinth_03.svg" -->

Note:
* API Owner can create a manifest out of all APIs it owns containing 1-n endpoints.
* This manifest is a proof of being allowed to expose the endpoints within, not a proof that the 
  instance presenting it also implements the endpoints.

^^^

## Enter Endpoint Discovery

<!-- .slide: data-background="images/network-landscape/network-landscape-endpointdiscovery.svg" -->

^^^

## Just register?

<!-- .slide: data-background="images/network-landscape/network-landscape-register.svg" -->

Note:
* I think not
* Terribly insecure
* Passwords and ACLs are hard to maintain because they are not semantic

^^^

<!-- .slide: data-background="images/v4/registry.svg" -->

^^^

<!-- .slide: data-background="images/v4/menu_and_receipt_02.svg" -->


^^^

<!-- .slide: data-background="images/v3/Labyrinth_01.svg" -->

^^^

<!-- .slide: data-background="images/v3/Labyrinth_02.svg" -->

^^^

<!-- .slide: data-background="images/v1/Labyrinth_04.svg" -->


^^^

## Registration

<!-- .slide: data-background="images/network-landscape/network-landscape-registerwithmanifest.svg" -->

^^^

## Connect A & B
<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab.svg" -->

Note:
* In order to be resilient we use Finagle for load-balancing and retries.
* We retry automatically when it is safe according to REST guidelines

^^^

## Securely ...

<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab-mtls.svg" -->

^^^

<!-- .slide: data-background="images/v4/menu_and_receipt_01.svg" -->

Note:
* Subscribe to any positive number of endpoints. When approved you obtain your PeerToken
* This PeerToken is prove that your *Service* is allowed to consume the *endpoints* mentioned
  within. 

^^^

## Present PeerToken

<!-- .slide: data-background="images/network-landscape/network-landscape-connectionab-with-peertoken.svg" -->

^^^

## API Gateway

<!-- .slide: data-background="images/network-landscape/network-landscape-apigateway.svg" -->

Note:

* Skipping all the details regarding gateways as they warrant an dedicated talk each


^^^

## Authentication

<!-- .slide: data-background="images/v2/ID_01.svg" -->

Note:
* Multiple means, username/pw, mobile, etc.


^^^

## Authentication

<!-- .slide: data-background="images/network-landscape/network-landscape-accesstoken.svg" -->

^^^

## Permissions

* Thousands of business rules
* Open vs Closed questions
    * What can I do?
    * Can I do X?
* Very hard to distribute per API
* Highly scaled and distributed Central API

Note:

* Hard to implement per api because of complexity, organisational orchestration

^^^

## Authentication

<!-- .slide: data-background="images/network-landscape/network-landscape-accesstoken.svg" -->


^^^

## Permissions API

<!-- .slide: data-background="images/network-landscape/network-landscape-permissionsapi.svg" -->

^^^

## Confidence Checking

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-deployment.svg" -->

Note:
 *   Backwards Compatibility       
    *   Client: Don't break on extra data in response
    *   Server: Don't break on extra data in requests
* Our default client is implemented using this

^^^

## Confidence Checking

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-request.svg" -->

^^^

## Confidence Checking

<!-- .slide: data-background="images/network-landscape/network-landscape-confidencecheck-success.svg" -->


^^^

## Observability

<!-- .slide: data-background="images/network-landscape/network-landscape-monitoring.svg" -->


^^^

## Useful metrics

* `failures`
* `scheduler/blocking_ms`
* `loadbalancer/size` == `loadbalancer/busy`
* `retries`
* See [metrics docs](https://twitter.github.io/finagle/guide/Metrics.html)

===

<!-- .slide: class="center" -->
# Finagle extensions

Note:
* Enough with all those pictures
* Finagle is already quite resilient by default but can be more performant
* We added some things particular to our stack and our java consumers
* Caveat: all code is pretty low-level, no cats inside :)

^^^

## Global API SDK

* Used by ING teams globally
* Primarily developed in Scala
* Serves both Java and Scala development teams

^^^

## Why Finagle

* Out of the Box
    * Performant
    * Load-Balancing
    * Retries
    * Circuit-Breaking
* Easily extensible:
    * Metrics
    * Endpoint Discovery
* Towards Scala with Finagle @ Scala Exchange 2015 ([slides](http://spockz.github.io/towards-finagle/), [video](https://skillsmatter.com/skillscasts/6953-towards-finagle-at-ing-bank))


^^^

## Use Response Classifiers

* Teach Finagle about functional errors
* Partial Functions

Note:

* Partial functions are not so nice in Java

^^^

## Java-Friendly Response Classifiers

```scala
@FunctionalInterface
trait AbstractHttpResponseClassifier extends PartialFunction[ReqRep, ResponseClass] {
  def define(request: Request, result: Try[Response]): Optional[ResponseClass]
}
```

```scala
def constantClassifier(responseClass: ResponseClass): ResponseClassifier 

def scopeToRequest(requestScope: Request => Boolean,
                   classifier: ResponseClassifier): ResponseClassifier

def scopeToResponse(responseScope: Response => Boolean,
                    classifier: ResponseClassifier): ResponseClassifier

def scopeToThrow(throwableScoping: Throwable => Boolean,
                 classifier: ResponseClassifier): ResponseClassifier

@varargs
def classifyHttpResponseStatusAs(classification: ResponseClass)
                                (statuses: Status*): ResponseClassifier

@varargs
def classifyHttpResponseCodesAs(classification: ResponseClass)
                               (statusCodes: Int*): ResponseClassifier
```

^^^

## Stack Enhancements
<!-- .slide: data-background="images/clientstack-apisdk.svg" -->

Note:

* Automatic Addition of Peer Tokens
* Tracing integration with OpenTracing
* Finagle uses the Response Classifiers for metrics and unavailability, but not for retry.
* Debug module for printing exactly what goes over the wire, useful when using TLS, doesn't require mITM.

^^^

## Endpoint Discovery integration

* <!-- .element: class="fragment" --> Finagle uses `Resolver`s for finding instances
* <!-- .element: class="fragment" --> `Resolver::bind(arg: String): Var[Addr]`
* <!-- .element: class="fragment" --> `Var[Addr]` is an updatable set of `Addr`
* <!-- .element: class="fragment" --> `Addr` can be `Pending`, `Negative`, `Failed`, or `Bound`
* <!-- .element: class="fragment" --> `Bound` contains a set of addresses with metadata

```scala
Http.client.newService("phoenix!api.ing.com:GET:/accounts/{accountId}")
```
<!-- .element: class="fragment" --> 

Note:
* Re-using the HTTP client to call Endpoint Discovery and use the results
* Bootstrapping down by initiating the Name/Var

^^^

## DeferringResolver

* Finagle's Resolvers are loaded with Service Loading
* Java's Service Loading is not idiomatic to Spring Boot
* Created `DeferringResolver` to buffer queries and switch streams to a to be set actual resolver.

Note:

* Example of another thing to think of when serving a wide set of customers

^^^

## Hostname Verifier

```scala
/**
 * Modified version of [[com.twitter.finagle.ssl.client.HostnameVerifier]] which always 
 * checks against the hostname from the [[Address]] instead of a statically 
 * configured parameter.
 *
 * @param checker Implementation of [[HostnameChecker]] for verifying whether the 
 *                certificate was issued to the specific instance.
 */
class HostnameVerifier(checker: HostnameChecker) extends SslClientSessionVerifier  {
  ..
}

Http.client.configured(SslClientSessionVerifier.Param(..).mk());
```

Note:
* We will be switching to Service name validation in the future

^^^

## Powerful Delegation Tables (DTAB)

> With great power comes great responsibility — [QI](https://quoteinvestigator.com/2015/07/23/great-power/)

<pre>
<code data-trim data-noescape class="bash">
/paymentService => /$/inet/<strong>mymalicious.host</strong>/1337 # bad
</code></pre>
<!-- .element: class="fragment" --> 

<pre>
<code data-trim data-noescape class="bash">
/paymentService => /paymentService/<strong>my-explicit.host</strong>/1337 # Good</code></pre>
<!-- .element: class="fragment" --> 

^^^

## Mutual TLS authentication


```scala
  def createMutualTlsContext(keyStore: InputStream, keyStorePassphrase: Array[Char], privateKeyPassphrase: Array[Char],
                             trustKeystore: InputStream, trustKeystorePassphrase: Array[Char]): SSLContext = {
      require(Option(keyStore).isDefined, "Client keystore must be defined")
      require(Option(trustKeystore).isDefined, "Trust store must be defined")

      // First initialize the key and trust material
      val ksKeys = KeyStore.getInstance("JKS")
      ksKeys.load(keyStore, clientKeystorePassphrase)
      val ksTrust = KeyStore.getInstance("JKS")
      ksTrust.load(trustKeystore, trustKeystorePassphrase)

      // KeyManagers decide which key material to use
      val kmf = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm)
      kmf.init(ksKeys, clientKeyPassphrase)

      // TrustManagers decide whether to allow connections
      val tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm)
      tmf.init(ksTrust)

      val sslContext = SSLContext.getInstance("TLS")
      sslContext.init(kmf.getKeyManagers, tmf.getTrustManagers, null)
      sslContext
  }
```

^^^

## Mutual TLS authentication continued

###### <!-- .element: class="fragment" --> `<=` 18.2.0 && Netty3, Netty4 HTTP 1 

```scala
val clientSSLContext = createMutualTlsContext(..)
Http.client.withTransport().tls(clientSSLContext)
```
<!-- .element: class="fragment" -->

###### <!-- .element: class="fragment" --> `>=` 17.10.0 && Netty 4 (HTTP2)
```scala
val sslClientConfiguration =
        SslClientConfiguration(None(),
          KeyCredentials.CertAndKey(new File(???), new File(???)),
          TrustCredentials.CertCollection(new File(???)),
          CipherSuites.Unspecified,
          Protocols.Unspecified,
          ApplicationProtocols.Unspecified)

Http.client.withTransport.tls(sslClientConfiguration)
```
 <!-- .element: class="fragment" -->
###### <!-- .element: class="fragment" --> `>=` 18.6.0  ([Hopefully](https://github.com/twitter/finagle/pull/693))
```scala
 val sslClientConfiguration =
      SslClientConfiguration(None(),
        KeyCredentials.KeyManagerFactory(kmf),
        TrustCredentials.TrustManagerFactory(tmf),
        CipherSuites.Unspecified,
        Protocols.Unspecified,
        ApplicationProtocols.Unspecified)

Http.client.withTransport.tls(sslClientConfiguration)
```
<!-- .element: class="fragment" -->


===
<!-- .slide: class="center" -->
# Lessons Learned

^^^

## Education

* *Know* what happens under the hood
* But *don't feel* it

Note:
* Even though things should be easy, teams should know what happens under the hood
  and be able to finetune.
* Keep educating, teams change, best practices change, libraries change.
* Obtaining maximum performance with maximum resilience is quite a detailed job

^^^

## Don't forget those time-outs

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

## Beware of downing instances

* Functional Errors may occur for a small number of customers only
* Downing instances can lead to unavailability for other customers as well


^^^
<!-- .slide: class="center" -->
## L7 Load-Balancers 

Note:

* Be wary when other load-balancers enter the stack. Unexpected behaviour might occur. Has effects on retries and failure accrual.
* Disable failure-accrual, failfast, and possibly retries

^^^
<!-- .slide: class="center" -->
## Stick to REST

Note:
* Developers will make assumptions; Better not to stray from the standard

^^^
<!-- .slide: class="center" -->
## Keep everyone up-to-date

Note:
* Fixes bugs
* Improves performance
* Enables you to activate H2 / new features

^^^

## Local Development

* Very nice: Reproduce retry & load-balancing issues locally
* mTLS everywhere complicates matters

===
<!-- .slide: class="center" -->
# Additional measures

^^^

## Additional measures

* Static Secure Code Analysis
* OWASP Dependency check
* Docker Image/Container scanners
* Live vulnerability scanning
* Penetration Testing
* Patching
* ...


===
<!-- .slide: class="center" -->
# Future Work

^^^
## Future work
*   Store SLA in PeerToken
*   Support concurrent Live API Specs
*   Move parts to Kubernetes
*   Investigate [Tunables](https://twitter.github.io/finagle/guide/Configuration.html#tunables)
*   HTTP2   
*   Investigate gRPC


===
<!-- .slide: class="center" -->
# Concluding

^^^

## Summary

* Security and Resilience start with Architecture, succeeds with implementation
* Team Education is important
* Do Monitoring
* Maintaining a Scala Library used by Java products is challenging

^^^

<!-- .slide: class="center" -->
# Thank You

<small> Yes, we are <a href="https://www.ing.jobs/Global/Careers/Job-opportunities.htm?jobfield_name_level_1=Information%2BTechnology">hiring</a> :)</small>
