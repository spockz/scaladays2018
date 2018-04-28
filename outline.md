**Messsage**: Managing, registering, discovering, exposing, and consuming APIs, securely and resilient *is* possible.

* Myself
    1.   Father of beautiful almost 2 year old daughter
    1.   Functional Programming Enthousiast
    1.   Lead Developer Global API SDK @ING
* Introduction
    *   Context 
        *   ING Group, developer teams & countries
        *   (When did we start) 
        *   Deployment process, CDaaS pipeline, out-of-scope for here
    *   Founding Principles
        *   Global Addressable & Routable name/ip space
        *   Consuming APIs should be as convenient for Internal Developer as for an external one.
            * Finding instances, load-balancing, up-time, etc.
        *   Set of modular, cherry-pickable components and an off-the-shelf framework providing SDK for Global, 
            *   Ofts framework Spring-Boot
            *   Stick with framework idiomatic ways
            *   Note: Makes service loading hard to use
        *   Everything should sufficient => Signed AT and PT
            *   Touch on Permissions api that we cannot do that everywhere
    *   Mission Statement
        *   Help teams that build apis deliver business value
            *   Securely
            *   Resiliently
            *   Expose and Consume with minimal effort
*   Terminology: The Meta-Model
    *   API, Endpoint, Service, Service Version, Instance etc.
    *   Quickly, point to Patrice's presentation
*   Developer Overview
    *   Principles:
        *   Global namespace: names and routing
        *   Developers internally as easy as externally:
            *   Downtime
            *   Finding instances by endpoint in contrast with service discovery
            *   ?
        *   Secure with mTLS
        *   REST/HTTP/JSON
    *   Big Picture including Dev Portal + Connections + ED + Tracing + Logs
        *   Mandatory, however robust it is, you still need observability for everything you didn't catch

    *   Start building up the picture in the next slides
*   Releasing
    *   Backwards Compatibility       
        *   Requirement:
            *   Client: Don't break on extra data in response
            *   Server: Don't break on extra data in requests
    *   Confidence checking via dtab
        *   Block InET resolvers and from external
        *   Thinking of signing the dtab header by the gateway
    *   **Note**: In practice we already allow multiple services to implement the same spec as long as the spec owner approves
    *   We do filter on spec versions so we only use compatible instances
    *   A/B Testing of code not covered here, but mention we don't use routing for it
    *   Ambition:
        *   Token for subscribtions externally so we can naturally migrate running systems
*   Resilience
    *   Use signed access tokens and peer tokens to avoid runtime dependency on Sessions DB and Developer Portal
    *   Finagle pretty good out of the box, can be better (esp. quicker and more your own program view)
    *   Setting timeouts! + code
    *   Use Response Classifiers
        *   Java friendly classifier
    *   Response Classification Filter Transforming
    *   PhoenixClient + Spring Boot => DeferringResolver
    *   Education, education, education
    *   Lessons learned: 
        *   You really don't want opaque load-balancers
        *   Stick to the REST standards, everyone, yes that also means you. :)
        *   Educating teams is hard, make very good defaults
            *   Teams change
            *   Best practices change
            *   Quite detailed
        *   Keep everyone up-to-date 
            *   Versions bugs etc
            *   Performance + features like http2
    *   
*   Security

    *   Identity keystore is injected at deploymenttime
    *   Code for setting up mutualTLS connection
    *   ClientSessionVerifier => ServiceNameVerifier
    *   PeerTokens
    *   Confidence Checking only allow instnaces known to Endpoint Discovery
        *   Block $inet resolver
*   Future
    *   SLA in PeerToken
    *   Support concurrent Live API Specs
    *   We are not blind man, eventhough bank! Integration with Kubernetes (near!)
    *   Investigate tunables, which conflict with immutable containers somewhat
    *   Idempotent POSTs with UUIDs and framework-provided 'transactions' and use nack headers
    *   HTTP2   
    
    
ToDo Mentions:

* Integrating Gateways is hard
    * Custom instances
    * Different concerns: on which host exposed etc.
* Manifest = proof of allowed to host, not proof of implementation    
* Local Development Support is good to have
* Trade-off between changeavility and stability by allowing non breaking changes to APIs. => need to stick to best practices