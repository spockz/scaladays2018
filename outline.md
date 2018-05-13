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
            *   Off the shelf framework Spring-Boot
            *   Stick with framework idiomatic ways
            *   Note: Makes service loading hard to use
        *   Everything should be self-sufficient => Signed AT and PT
            *   Touch on Permissions api that we cannot do that everywhere
    *   Mission Statement
        *   Help teams that build apis deliver business value
            *   Securely
            *   Resiliently
            *   Expose and Consume with minimal effort
*   Terminology: The Meta-Model
    *   API, Endpoint, Service, Service Version, Instance etc.
    *   Quickly, point to Patrice's presentation : https://www.youtube.com/watch?v=RvcDs_JLc0Y
*   Landscape
    *   Key Principles:
        *   Global namespace: names and routing
        *   Developers internally as easy as externally:
            *   Downtime / releases
            *   Finding instances by endpoint in contrast with service discovery
            *   ?
        *   Everything should self-sufficient => Signed AT and PT
            *   Touch on Permissions api that we cannot do that everywhere
        *   Secure with mTLS (Zero Trust Networking)
        *   REST/HTTP/JSON
    *   Start building up the picture in the next slides
        * Just API X
        * Add the implementing service(S!) A
        * Say: Having something running is not good enough, you need a consumer
        * Add Service B
        * Needs to Know where to find API X
        * Picture of Lady before doolhof
        * Asks registry 
        * Gets answer
        * Add Endpoint Discovery and lookup line
        * Isn't going to work if Service B instances aren't registering themselves
        * Show the two ladies registering their instance at ED
        * Add the line for registering
        * If it was this easy everyone could register and hijack payments
        * Registry image
        * Picture with guy selecting endpoints and getting the manifest
        * Add the manifest on the registration line
        * Line between B and A
        * Add mutualTLS lock
        * Guy selecting the peertoken
        * Big PeerToken image explain what it does
        * Add PeerToken on line SVC B -> SVC A
        * Add Gateway with www cloud, site, and api domain
        * Add Access Token slide explaining what it is; skip the how to identify but say multiple means
            * JWE
        * Pass the access token along browser/gw/Svc B/Svc A
            * Gateway does coarse grained check based on scopes
        * Slide with different amount of products context
        * Introduce permissions API which checks permissions
        * Add slide with all the monitoring stuff
        * Remove all the monitoring stuff
        *   Releasing
            *   Backwards Compatibility       
                *   Requirement:
                    *   Client: Don't break on extra data in response
                    *   Server: Don't break on extra data in requests
            *   Confidence checking via dtab
                *   Block InET resolvers and from external
                *   Thinking of signing the dtab header by the gateway
            *   **Note**: In practice we already allow multiple services to implement the same spec as long as the spec owner approves;
            *   We do filter on spec versions so we only use compatible instances in the off chance that an older version comes back to life.
            *   A/B Testing of code not covered here, but mention we don't use routing for it
            *   Ambition:
                *   Token for subscriptions externally so we can naturally migrate running systems
*   Coding wise
    *   Resilience
        *   Mention: Use signed access tokens and peer tokens to avoid runtime dependency on Sessions DB and Developer Portal
            * This means that it should also be part of the architecture
        *   Finagle pretty good out of the box, can be better (esp. tighter timeouts and more your own program view) (? What was this about again???)
        *   Setting timeouts! + code
        *   Use Response Classifiers
            *   Java friendly classifier
        *   Response Classification Filter Transforming
            * Watch out for downing instances because of functional errors, it might cut off traffic for normal customers because of one bad apple
        *   PhoenixResolver + Spring Boot => DeferringResolver
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

        *   Identity keystore is injected at deployment time
        *   Code for setting up mutualTLS connection
        *   ClientSessionVerifier => ServiceNameVerifier
        *   PeerTokens
        *   Confidence Checking only allow instnaces known to Endpoint Discovery
            *   Block $inet resolver
*   Future
    *   SLA in PeerToken
    *   Support concurrent Live API Specs
    *   We are not blind man, even though bank! Integration with Kubernetes (near!)
    *   Investigate tunables, which conflict with immutable containers somewhat
    *   Idempotent POSTs with UUIDs and framework-provided 'transactions' and use nack headers
    *   Moving towards HTTP2   
    *   gRPC
    
    
ToDo Mentions:

* Integrating Gateways is hard
    * Custom instances
    * Different concerns: on which host exposed etc.
* Manifest = proof of allowed to host, not proof of implementation    
* Local Development Support is good to have
* Trade-off between changeability and stability by allowing non breaking changes to APIs. => need to stick to best practices