# Player roles

Player roles in Deployments & Disasters mimic the class / race concepts in traditional tabletop RPGs. This document offers examples of few premade roles. Unlike traditional RPGs each role differs in information they receive at the beginning of each session.

When referring to real world usage in this document that includes tools and technologies that your players might encounter on the job. By introducing real world tools into the game players are encouraged to learn about them and improve their performance. See the [design.md](./design.md) document for more info.

If you find any of these roles doesn't fit your real world procedures / experiences, feel free to change them up. Just keep in mind that they are separated and limited in information they receive for a reason.


## Frontend developer

They should receive information on the architecture of the frontend layer of the system, like language and frameworks used to develop it, main used libraries, programming approach used, how modules are structured, how app is deployed, etc.

Their tools proficiency includes:

* JavaScript, React, Angular, Node.js, any language / framework used in real world 
* NewRelic or monitoring tools you use in real world
* Redis or caching solution used in real world

## API developer

They should receive any information on the architecture of the public API layer of he system. This includes load balancers, reverse proxies, API Gateways and actual applications that implement APIs.

Their tools proficiency includes:

* Nginx, HAProxy or any balancer used in real world
* Language used to implement the APIs
* Prometheus or equivalent metrics system

## Backend developer

They should receive any info related to the architecture of the backend components of the system. This could include individual microservices, messaging / queueing solutions, databse details, etc.

Their tools proficiency includes:

* Language used to write the microservices
* Management skills of used messaging / queueing  solutions
* SQL and other used DB technologies (like no-sql)
* Graylog / Kibana or other logging solution

## SRE

They should receive information on logging, metrics and alerting setup on all components of the system. They don't know how all the stuff works, but they should know how to check if it's working. They also know basic disaster recovery procedures, such as where circuit breakers are, how to restart components, how to ban users, etc.

Their tools proficiency includes:

* Where to find any metric / log
* Query languages used by metrics / logging systems
* Docker, Kubernetes or other deployment tools used in real world

## Project lead

Their role is to coordinate the rest of the team and provide a more business focused perspective. They know what each other team members' role is, so they can coordinate between them. They receive info on key clients and their use cases. They also know how system experiencing the incident relates to other systems within the company.

Their tools proficiency includes:

* CRM system that's used in real world
* Access to other NPCs within the company (like account managers)
* NewRelic or similar analytics tools

