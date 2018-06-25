![ixo Logo](./img/ixo-Cyan@2x.png)
# Welcome to the ixo Protocol
Please have a look at the [ixo Foundation website](http://ixo.foundation) for more information on what we do.

#Introduction
The documentation on this site is aimed at technical people who wish to understand the inner workings of the ixo protocol, how it works and how to integrate your applications and systems into the protocol.


In simple terms, the ixo protocol allows for the creating of projects in persuit of the UN's sustainable development goals. People, systems or IOT devices can then submit claims regarding 
the work performed against these projects.  This work is then evaluated for accuracy and authenticity by evaluators.  Evaluators may also be people, systems or evaluation oracles that could pull in vast amounts of data to determine the evaluation result.

#Overview of the Architecture
The ixo platform is managed by a number of components and systems that interoperate in order to provide the full services that the ixo protocol offers.

![Architecture Overview](./img/architecture-06-2018-Overview.png)

For futher information see the [documentation on our components](./documentation/).

##The ixo Blockchain
The ixo blockchain contains the records of evey claim that is issued against a project the the subsequent evaluation of those claims.  Each record is validated by a quorum of validator nodes before it is written to the chain and thereafter the record cannot be removed or updated.  This data is then aggegated to build out the final states of the projects.

All the information on the ixo blockchain is publically avaialble through the ixo Explorer. This data includes the project information, stats regarding the project, the stakeholders of the project and the structure of data being captured against the project.  All actual claim data is stored in the Project Datastore.

##Privacy of Data
Much of the data that is captured within a claim is highly sensive in nature.  This data might have specific regulator requirements such as GDPR or maybe the data may not reside outside certain geographical boundaries.  In order to comply with this and to also put the data into the hands of the owners of the project we have created the concept of independant data stores for each project.

The ixo Blockchain keeps a link to the location of these project data stores and provides services to the project data that are goverened by cryptographic access controls.

##Security
All requests to that create data or access sensitive data require cryptographic signatures and a capabilities model supports this to provide finer grained access control.

# Pilots
## Amply
Amply is a MVP project that we have built to prove the utility of the ixo protocol.  The code for this project can be found on our [Amply github repository](https://github.com/TrustlabTech).  



