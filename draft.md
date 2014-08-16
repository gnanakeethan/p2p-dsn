#P2P-DSN Specification v-1.0 (version negative one, alpha)

Peer-To-Peer Communication protocol emphasizes on a general unified suite with various Peer-To-Peer secure and authentication methods to provide Peer-To-Peer decentralized communication systems, regardless of underlying protocols. The protocol is supposed to be an abstraction over the data. The possible uses of this layer, we wish to implement, are,
1. Remote file systems/seamless file transfer and accessing,
2. Remote streaming over a network, including web cameras, voice,
3. Providing necessary communication systems for instant messaging.
4. Bouncing encrypted data over various voluntary relays in case the other end is not reachable.

Network
=======

Connection
==========

A server is expoected to be running constantly over each peer such that it is open for connection requests. The connection may be over TLS or Plaintext as an additional encrypted authentication-based layer is to be used.


Authentication
==============

A is assumed to know B, therefore B's public key, or, B is to be verified by a trusted third-party C, in which user of A is to be asked whether B is to be trusted based on C's signatures.

Proposed authentication methods are to be commented on.

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

MSGn = Random message.

A establiches a connection to B. A sends MSG1 encrypted with B's public key. B decrypts it using B' and,
{1}
Sends back MSG1 encrypted using A, which A shoulc be able to decrypt using private key A'. In this case, B is verified. This should be repeated by B to A to verify A.
{2}
Sends back MSG1 + MSG2, encrypted using A's public key; A is able to decrypt it using A'. As A gets MSG1 + MSG2, A compares MSG1 with data of length of MSG1 from MSG1 + MSG2. If it is successful, A returns MSG2 encrypted with B's public key, and sends it to B, where B would decrypt it and compare it to original MSG2. MSG2's successful comparison means that it was actually A. (Not taking the statistically insignificant random matches or birthday problem in account)


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



Modification History
====================

Rishikedhan, 22-Jul-14<=>23-Jul-14, Created this draft.

Appendix A : License
====================

FreeBSD Documentation License.

Copyright 2014 (Rishikeshan) <email> <signature?>,

(Gnanakeethan) <gnanam@boun.cr> <signature?>,

(Your-contributor-name) <email> <signature?> et al <list link>. All rights reserved.

Redistribution and use in source and 'compiled' forms (SGML, HTML, PDF, PostScript, RTF and so forth) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code and compiled forms must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, PostScript, RTF and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY THE FREEBSD DOCUMENTATION PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE FREEBSD DOCUMENTATION PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
