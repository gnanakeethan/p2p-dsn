#P2P-DSN Specification v-1.0 (version negative one, alpha)

Peer-To-Peer Communication protocol emphasizes on a general unified suite with various Peer-To-Peer secure and authentication methods to provide Peer-To-Peer decentralized communication systems, regardless of underlying protocols. The protocol is supposed to be an abstraction over the data. The possible uses of this layer, we wish to implement, are,

1. Remote file systems/seamless file transfer and accessing,
2. Remote streaming over a network, including web cameras, voice,
3. Providing necessary communication systems for instant messaging.
4. Bouncing encrypted data over various voluntary relays in case the other end is not reachable.

This would effectively reduce the separate requirements for separate methods of communications between different networked applications.

#Network

Connection
==========

A server is expected to be running constantly over each peer such that it is open for connection requests. The connection may be over TLS or Plaintext as an additional encrypted authentication-based layer is to be used. The software has a "Listener" that "Listens" to a method. It could be SOCKETS, FILE, SHM, etc.

P-server is the software that manages data and all other outwards connections.


Peernames
=========

Data structure of peernames:
Public Key,
A short-name indicating their shortname,
User data,
A matrix of:
A method identifying the method how it was received,
A method-data field that contains additional information about the method abovementioned.

Apart from the convenience in the interface, Public Key should ALWAYS be used to identify/refer to users as short-name could be changed.


Authentication
==============

A is assumed to know B, therefore B's public key, or, B is to be verified by a trusted third-party C, in which user of A is to be asked whether B is to be trusted based on C's signatures.

Proposed authentication methods are to be commented on.

```
Public Key.
|===|
| a |
|===|
Public key = A,
Private key A'

|===|
| b |
|===|
Public key = B,
Private key B'
```

MSGn = Random message.

A establiches a connection to B. A sends MSG1 encrypted with B's public key. B decrypts it using B' and,
{1}
Sends back MSG1 encrypted using A, which A shoulc be able to decrypt using private key A'. In this case, B is verified. This should be repeated by B to A to verify A.
{2}
Sends back MSG1 + MSG2, encrypted using A's public key; A is able to decrypt it using A'. As A gets MSG1 + MSG2, A compares MSG1 with data of length of MSG1 from MSG1 + MSG2. If it is successful, A returns MSG2 encrypted with B's public key, and sends it to B, where B would decrypt it and compare it to original MSG2. MSG2's successful comparison means that it was actually A. (Not taking the statistically insignificant random matches or birthday problem in account)
{3}
Optional authentication and signing from a centralized server. Here, the peers are verified of authentication by a centralized server. The centralized server then may share a permanent or temporary symmetic-key between two peers.


Authentication identifiers:

AUTH {n|_}{s|a|_} ALGORITHM DATA

Here, "n" is the method number, s is symmetric key and a is assymetric key. Unsupported authentication menthods should be reported to peer which tries to connect.
_ => Cycle to next availabe methods-enumerator.

TPAs - Third-Party Verification.

The authentication information and verification, if not already avilable, could be provided by a trusted third-party verifier/authenticator, for a particular username and its corresponding Public-Key. This could partially centralized, however. The verifier signs the USERDATA with certain amount of LEVEL OF TRUST. The verifier could be just a server or peer and the userdata should be transferrable to others - the USERDATA and corresponding public key and signatures so that the verification could be "additive". A peer may have some additional signature and it should be added to others as long as it is valid, to the user's preference, of course.




Connection
==========

Connection could be chosen to be encrypted on totally public-key methods which are slow or on pre-shared (over Once-Public key agreement) per-session symmetric key methods, for which there are hardware implementations boosting performance.

Symmetric Key methods




Contact Data
============

Contact and Pseudo-contacts
===========================

Contacts are in traditional sense, friends authenticating with a known private key.

Pseudo-contacts are not contacts themselves. They are similar to "groups" found on various networks. Pseudo-contacts have their own key - recommended to be, but not necessarily, symmetric. These keys are to be shared with ALL of contacts belonging to the particular pseudocontact. Intended to be re-used among public contacts in that pseudocontact.

Private and public contacts

All members in pseudocontacts know that they are in the pseudocontact and also know the other public contacts in that particular pseudocontact.

Private pseudocontacts

Private pseudocontacts are expected not to be known to others in the pseudocontact by the creator of the pseudocontact. The communication using the key of the pseudocontact to other peers is, by default, to be restricted, as the person to be contacted might be able to find out that the mentioned private contact is a "private" contact of the pseudocontact.




Data
====

The public store and the private store
======================================

To keep things from getting too complicated, all the things to be exchanged except authentication are to be considered data in this section. They (any source of data) necessarily fall in either group.

Public store
============

To simplify, Public store is the store that is intended to be accessible to everyone. To make operations easy and fast, it might have files to be shared to the pseudocontact, in encrypted form with metadata on methods and intended readers/recipients, for example.

Private store
=============

Private store is "personalized" per contact or key. Camera access, for example, falls under private store, generally, as a few would like to make their camera streaming data available to everyone.

Relay functions may fall under either, as someone may choose to be a public or private relay.

Data Transmission
=================

Data should be separated based on the (1) connection type, (2) connection type-specific connection identifier, (3) a randomly assigned fixed length identifier first reporting length of identifier or "_" in case of any further annexations; a pre-agreed value. This pre-agreed value is constant, at any time, to a fixed peer connection service, while on connection.

The randomly assigned fixed-length identifier should be unique for that particular (1) connection type, (2) connection type-specific connection identifier.

The data is to be separated and given to the so-called underlying software.

Data is to be given to underlying software without the headers.

Eg:-
For web camera streaming,
HDHKGHSKGFHG [DATA]
The connection type could be IP.
The connection specific identifier is IP address.



Services and permissions.
=========================

Services are applications which use this protocol for whatever purpose. Service is a P-client. Each service has its associated pseudopermission/permission system. The system rejects connection if it is against the permission rule. Permissions are set for each contact/peer/pseudocontact, separately, with allow/deny. Permissions have these flags:
Allowedbydefault, Rejectedbydefault; Users may have one Takesprecedence.

Takesprecedence means it takes precedence over any other permissions.

Structure:
Permission data
VideoCall Rejectedbydefault
VideoConference Rejectedbydefault
DesktopShare Rejectedbydefault

Pseudocontact Friends
Member Ris

User data
Friends VideoConference Allow
Ris VideoCall Allow
Ris VideoConference Deny Takesprecedence

This rejects videocall from Ris.

In dilemma, defaults apply; Takesprecedence applies to a permission/pseudopermission to a user only once. Duplicate entries should not exist.


Each service has their handlers in Handlerdata, as incoming data can not be delivered sometimes in case service is not running.
Structure
Handlerdata
VoiceCall CallClient.exe


Data Availability to services.

For each connecting Service, there will be a connection specific identifier. The service shall not know that identifier. Service receives the data only, for that connection. On the service side,

Connect peer Kim.
Request service VideoCall.
Transfer data.

While, to the implemented P2PDSN server,
Connect peer Kim.
(Incoming request VideoCall)
Service check VideoCall.
(Service OK)
Assign a Random CSID.
Send data to Kim's P-server on that CSID.
Any data received, remove all the CSID and the data before it, deliver it to the Service by using IPC methods.

The client library allocates an IPC for each CSID. The client library does not know the CSID. Known information ever are, Peer name, Connection sequence ID, Data. The IPC is initialized by making a request on IPC initialization stream.





Modification History
====================

Rishikedhan, 22-Jul-14<=>23-Jul-14, Created this draft.

Edited, somewhere in between.

Rishikeshan, 02-Aug-14<=>03-Aug-14, Added Services and Permission, Data Avasilability to Services section, etc.

Rishikeshan, 16-Aug-14, Added Peerdata and TPVs.



Appendix A : License
====================

FreeBSD Documentation License.

Copyright 2014 (Rishikeshan) <email> <signature?>,
(Your-contributor-name) <email> <signature?> et al <list link>. All rights reserved.

Redistribution and use in source and 'compiled' forms (SGML, HTML, PDF, PostScript, RTF and so forth) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code and compiled forms must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, PostScript, RTF and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY THE FREEBSD DOCUMENTATION PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE FREEBSD DOCUMENTATION PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
