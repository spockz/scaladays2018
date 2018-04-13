**Messsage**: Managing, registering, discovering, exposing, and consuming APIs, securely and resilient *is* possible.

* Myself
    1.   Father of beautiful almost 2 year old daughter
    *   Functional Programming Enthousiast
    *   Lead Developer Global API SDK @ING
* Introduction
    *   Context 
        *   ING Group, developer teams & countries
        *   (When did we start) 
        *   Global Addressable & Routable name/ip space
        *   Deployment process, CDaaS pipeline, out-of-scope for here
        *   Set of modular, cherry-pickable components and an off-the-shelf framework providing SDK for GLobal
    *   Problem
    *   Goal to run APIs
        *   Securely
        *   Resiliently
        *   Expose and Consume with minimal effort
*   Meta-Model
    *   Quickly, point to Patrice's presentation
    *   API, Endpoint, Service, Service Version, Instance etc.
*   Developer Overview
    *   Big Picture including Dev Portal + Tracing + Logs
*   Releasing
    *   Backwards Compatability       
*   Resilience
    *   Setting timeouts!
    *   
    *   Use Response Classifiers
        *   Java friendly classifier
    *   Response Classification Filter Transforming
    *   PhoenixClient & DeferringResolver
    *   Education, education, education
    *   Lessons learned: 
        *   You really don't want opaque load-balancers
        *   Stick to the REST standards, everyone, yes that also means you. :)
    *   
*   Security
    *   Code
    *   ClientSessionVerifier
*   Future
    *   SLA in PeerToken
    *   Support concurrent Live API Specs
    *   Mesh integration with Kubernetes (near!)
    *   Idempotent POSTs with UUIDs and framework-provided 'transactions'
    *   HTTP2   
    
    
ToDo Mentions:
* Integrating Gateways is hard
* Manifest = proof of allowed to host, not proof of implementation    
* Done: Deferred Resolver
* Local Development Support