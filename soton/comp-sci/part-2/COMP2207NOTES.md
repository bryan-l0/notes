\setlength{\parskip}{0pt plus 0pt minus 0pt}

# Networking - Application, Transport, Network, Link, Physical

#### Physical Layer: 
Standards for transmission of bits for each media type, e.g. coaxial cable, fibre optic, wireless... IEEE802.11 for wifi.

## Link Layer: Framing, Addressing, Error Detection/Correction, Flow Control, MAC, Ethernet, Wireless
- Framing: Encapsulates the network layer packets into frames, adding headers and trailers. Can use a FLAG byte vaule to mark start and end. Use escape byte if FLAG occurs in data, when receiving strip first escape byte. Escape an escape byte in data.
- Can implement flow control mechanisms to regulate sender and receiver data flow.
- Can handle ACKs and errors. E.g. 802.11n block acknowledgements.
- ARQ (Stop&Wait Automatic Repeat Request) - Send frame, wait for ack to send next frame.
- Can be pipelined, e.g. sending multiple frames before first ACK. 2 ways to handle: Go-back-N ARQ resends frames from the frame that ACK wasn't received. Selective-Repeat ARQ resends missing frames only.
- Detecting Errors: Parity bit (counts even/odd number of 1s). CRC (Cyclic Redundancy Check): Checksum field in frame, calculated by sender and receiver. IPv4 has checksum, IPv6 not.
- MAC (Media Access Protocol): Manages access to/from the physical layer. Defines rules for sharing physical media among multiple devices to avoid collisions and allow for fair access.

## Ethernet - Ethernet Theory, ARP, Topologies, Switching, MAC port learning, Building, VLAN
- Ethernet Theory: Ethernet frame is in link layer. 48bit source and destination MAC, 802.1Q optional tag for VLAN ID and frame priority. MTU usually 1500 bytes, 42 bytes minimum.
- MAC Address (Link Layer Address): Medium Access Control Address. 48 bits, extensible to 64 bits. 24 bits for vendor allocation, 24 bits assigned by vendor.
- Ethernet Switches: Receives ethernet frames and decides whether to forward frame, if so, the port. Learns ethernet address of hosts on each switch interface/port. Can store and forward ethernet frames with error checks (CRC check).
- ARP (Address Resolution Protocol): Used to determine destination MAC address. Broadcast to ff:ff:ff:ff:ff:ff to ask for all connected hosts, gets reply from intended recipient. Usually cached by sender for 15-45s. ARP messages are 224 bits. If MAC address is not in ARP cache, it sends ARP request.
- Message Types: Unicast (1-1), Broadcast (1 sender many potential receivers), Multicast (1-N), Anycast (1 sender nearest instance of receiver).
- Spanning Tree (802.1d): Detects loops and removes paths from bridging LANs together. Elects a root bridge and calculates shortest path to it.
- Virtual LAN (802.1Q): Ethernet frame has optional 12 bit value in 802.1Q tag. Multiple VLANs can be carried by a trunked uplink, to exchange traffic between VLANs with a single physical link

## Network (Internet) Layer: IP Protocol, Packet Switching, Routing, IP Addressing, Fragmentation and Reassembly, ICMP, IPSec
- IP: Uses logical addressing & routing to forward packets across networks. Packet switched, connectionless. Unreliable (no guarantees, best effort), destination routed (routing tables on routers)
- Fragmentation: Splits packets and reassembles later. Ethernet MTU 1500 bytes + 18 bytes overhead.
- IP Addressing: IPv4 - 4x8 bit addresses with min 20 byte 13 field headers, can fragment and reassemble at any router. IPv6 - 8x4x4bit addresses with 40 byte 8 field headers, can only frag/reass at sender/destination, uses Path MTU Discovery before sending.
- Netmasks: Specifies how many bits to use to identify local machines. Typically 1st address is reserved, last is broadcast address, 1 is used for router. IPv4 has variable length prefixes, IPv6 has min length of 64 bits.
- NAT: Allows for IPv4 private addresses (RFC 1918) to be exposed to the internet. Translates a public ip + unique port to a private IP.
- ICMP (Internet Control Message Protocol): Encapsulated in standard IP packets. IPv4 uses it for information and error messages, IPv6 also uses it for router advertisement and neighbour discovery.
- NDP (Neighbour Discovery Protocol): Maps IPv6 addresses on the local subnet to a MAC address. Uses ICMP and multicast. Replaces ARP and ICMP Router Discovery from IPv4. Defines 5 ICMPv6 packet types: Router Solicitation (host sends RS message to all-routers multicast address for info on all routers on network), Router Advertisement (router sends periodic RA message to all-nodes multicast address), Neighbour Solicitation (host sends NS messages to find the MAC address of neighbours), Neighbour Advertisement (NS reply), Redirect (router informs a host of a better next-hop for a particular packet, to update its routing table).
- SLAAC (StateLess Address AutoConfiguration): Allows a host to autoconfigure network settings without DHCP. RA specifies whether SLAAC should be used. Built by using 64bit prefix from RA, 64bit host generated part. RFC4862 - Generated based on host MAC address, with padding (MAC is 48 bits). RFC7217 - Uses pseudo-random function that takes prefix, network card identifier, network identifier, DAD counter, and secret key as arguments. 
- IPv6 Privacy Extensions: RFC2941 uses an ephemeral, periodically randomly generated host part for outbound connections, such that the IPv6 address isn't stable to improve privacy. Host still has SLAAC-configured address for inbound connectinos.
- DHCPv6 also exists if SLAAC is disabled, but assigns a IPv6 address to a DHCP Unique Identifier instead of MAC address due to the use of temporary addresses.
- Benefits of IPv6: Removes need of NAT and DHCP, more efficient routing and packet processing, fragmentation only at sender.

## Routing (Network Layer)
**Routing Table:** Hosts have a simple routing table, that may be built from DHCP/IPv6 RA. Usually includes the local subnet, and a default router. Priorities most specific route (smallest subnet), then a metric is used as tiebreaker (lower = more priority).  
**Autonomous System:** Larget network/group of networks with unified routing policy, identified with a unique ASN (AS Number) that is assigned to them.  
**Interior Gateway Protocols:** Used within an AS. Distance Vector - talks only to neighbouring routers, exchanges shortest route information for known prefixes (RIP). Link State - Talks to all routers to create overall topology, by having routers flooding info messages describing their connected neighbours across the entire network (IS-IS, OSPF).  
**Routing Information Protocol (RIP):** Uses hopcount as metric, sends entire routing table periodically to neighbours. Receiving routers update their best routes to a given prefix. Limitations: Low frequency, no ACKs, metrics are basic (hop count) and capped at 15, doesn't have knowledge of topology, authenticates with MD5.  
**Link State Routing:** Discover neighbours & calcs cost metric, flood message with this info to all routers, use received messages to build topology with Dijkstra's. Messages send periodically or when change in connectivity happens. Converges faster with OSPF's detection of change in topology, avoids loops due to knowledge of entire topology.  
**Exterior Routing Protocols:** Advertise network prefixes to neighbouring networks, can offer/not offer to transit to other networks. Policy is more important than path costs.  
**Border Gateway Protocol (BGP):** Distance Vector with extra info. Configure IP of neighbour and AS, create BGP peering session (TCP Port 179, send whole routing table then update periodically), advertise known routes to neighbours. Downsides: Relies on trust, costly, slow (default KEEPALIVE 60s HOLDWON 180s), limited BGP routing table sizes (1m IPv4, 128k IPv6).  
**Traceroute:** Sends packets with gradually increasing TTL, then checks ICMP time exceeded message. Routes are likely asymmetric, latency != RTT/2.

## Transport Layer
**UDP (User Datagram Protocol):** Connectionless, retransmission by application, low overhead (no connection and smaller header), constant bitrate possible. 4x16bit header, can multicast.  
**TCP (Transmission Control Protocol):** Uses connections, has acks/retransmissions, flow/congestion control. 10x16bit mandatory fields, optional 10x32bit flags. 3 stage handshake: SYN, SYN-ACK, ACK.  
**TCP Flow Control:** Sender sends packet with sequence number. Receiver replies with ack number with sequence number it is expecting, and available window size. If reply is not received within sender timer, resends packet. If ack indicates window size of 0, sender can either wait for a new window advertisement, or regularly ping it with 1 byte probes to get window advertisements.  
**TCP Congenstion Control:** Handled by sender. Indicates how many bytes the sender can put into the network at any time. Is used in conjunction with flow control window, sends a packet of bytes the size of the smaller window. Congestion window starts off with small numbers of MSS (maximum segment size), and is increased by 1 for each ACK received, doubling the window size every trip. Once the threshold is reached, increase window size slowly until timeouts occur, then repeat the slow start.  
**(De)multiplexing:** Converts multiple app's data into 1 stream to transmit, then separates data back into streams based on port.

## Application Layer - DHCP, SMTP, IMAP, HTTP, QUIC, File Server, DNS
- DHCP: Client sends a DHCP DISCOVER request, server replies with DHCP OFFER, client replies with DHCP REQUEST to select IP, server replies with DHCP ACK to acknowledge it. Expires usually every 24 hours, client renews it before it expires.
- SMTP (Simple Mail Transfer Protocol): Sender establishes TCP connection to mail server, replies with 220 "READY", sends "EHLO", replies with 250 "OK" + list of supported SMTP extensions. Mail transaction includes MAIL (sender email), RCPT (recipient email), DATA (start of message), QUIT (terminate SMTP session) + reply of 221. SMTP Authentication is an extension that requires sender to be authenticated.
- IMAP (Internet Message Access Protocol): Allows an email client to retrieve and manage emails from a mail server, as well as synchronised access from multiple clients. This also uses a TCP connection.
- HTTP: Text based TCP connection protocol on port 80. Allows for RESTful APIs to function. Main methods: GET, POST (Updates), PUT (Replaces), DELETE, gets a status code as response.
- QUIC (Quick UDP Internet Connections): Built on top of UDP, reduced latency compared to TCP.
- RTSP (Real Time Streaming Protocol): Uses RTP (Real-time Transport Protocol) to deliver media (with UDP usually). Uses a URL scheme with arguments.
- SMB (Server Message Block): File sharing protocol for file and printer sharing, and remote file access etc., in Windows based environments. Provides authentication and file locking, used for DFS (Distributed File System) in Windows.
- NFS (Network File System): Allows clients to access files and directories located on remote servers or over a network
- MQTT (Message Queuing Telemetry Transport): Uses a publish-subscribe protocol. Messages published to a broker, clients subscribe to data streams. Topics are hierachical, # is wildcard, + is a single level wildcard.
- CoAP (Constrained Application Protocol): HTTP-like protocol with less overhead that prefers UDP. Lightweight compared to MQTT. Binary formatted protocol (CBOR - Concice Binary Object Representation) to optimise payload size and reduce protocol overhead. Confirmable bit in ACK to deal with packet loss. Can deal with slow responses by sending ACK immediately, payload later. A proxy can be used to cache a response so the node can sleep. Can also publish-subscribe, client sends a GET request with "observe=0" to start observing the resource. Server increments the "observe" value everytime it publishes a notification for tracking loss. Nodes can push updated values. Block transfers are also supported, to be rebuilt at the client, by sending a "CON" request. Also supports multicast, client can broadcast query to multiple devices.
- DNS (Domain Name System): A network of DNS servers store and manage mappings of domain names to IP addresses. Client queries DNS resolver, which queries DNS root server if not in cache. Returns TLD server, which is queried to get the authoritative name server, which is queried to get the IP.

## Network Security - Defence in Depth over Security through Obscurity
- Firewalls: Network device that sits on the edge of network, that inspects each packet passing through and matches them to a set of rules. Small networks may have it on an embedded platform with other functions. In enterprise it might be combined with a router or standalone device. Stateless firewalls would allow all traffic to a specific port. Modern stateful firewalls inspect TCP/IP header fields and keep track of connections, and have rules for related and established, e.g. allow outbound traffic to port 80 but block inbound traffic to port 80 unless its related to existing connection. Common home configuration: Deny all inbound unelss related/established, allow all outbound. Application layer firewalls also exist that inspect the contents of the packet to ensure the packet matches its header, but may not work for encrypted traffic.
- Network Intrusion Detection (NIDS): Attempt to mitigate by sniffing the network for malicious activity or policy violations. Can monitor and alert, or even take action to prevent intrusion.
- Physical Security: Hard to prevent physical access from unauthorised users. Network Access Control can be used to prevent unauthorised devices from connecting to network, e.g. MAC address filtering, port based network access (802.1x). 802.1x uses EAP (Extensible Authentication Protocol) to authenticate clients, and blocks the switch port/wireless client until authentication. Involves 3 parties: client, authenticator (switch/AP), authentication server. However, NAP doesn't stop sniffing if they tap a connection. ARP cache poisoning can also be used. This is solved by IPSEC (IP Security: Encryption) at the network layer, that authenticates and encrypts packets.
- VPN (Virtual Private Networks): Simulates direct connection between devices. Site-to-Site VPN - securely connects 2 networks. Remote-access VPN: Individual clients connect to a VPN server.
- Wireless Security: Every client receives all traffic. WEP (Wired Equivalent Privacy) - 40/104 bit encryption key and 24bit IV. Weak encryption, static key, reused IV, no data integrity protection. WPA (WiFi Protected Access) - Uses TKIP (Temporal Key Integrity Protocol).Weak passwords are suspect to dictionary attacks, due to its pre-shared key. WPS is also a weakness, 8digit pin can be brute forced + physical security in button to use WPS. WPA2 - Uses AES as encryption. WPA3 - Improves security of initial key exchange. WPA-Personal - Uses pre-shared keys. WPA-Enterprise - Uses 802.1x authentication.
- DNS Exploitation: DNS Amplification - Spoofing source IP on DNS requests, victim gets flooded by DNS responses. DNS cache poisoning also possible by providing bogus response to caching resolver. DNS is also not encrypted, so your usage patterns can be sniffed through your DNS queries.
- DNS Improvements: DNSSEC (DNS Security Extensions) - Allows for signed responses, but with bigger responses, and complexity to manage signatures. Deployment in progress just like IPv6. Blocking spoofed IP doesn't help because it blocks victim not attacker.
- TCP SYN Flood: Send SYN packets to server, but doesn't send a final ACK back, so server is left waiting.
- HTTP Flood: Spamming POST requests. Uncomplete GET requests.

\newpage
# Distributed Systems

## Physical Models
Interconnected nodes of computers through a network, to communicate and exchange data. It also includes the rest of the infrastructure including networking devices.

## Architectural Models
Describes what components are involved, how they are related, and how they interact. Validated against requirements on availability, performance, adaptability, security.  

### Building Blocks
**Communicating Entities:**  
**Communication Paradigms:** Inter-process communication i.e. message passing and socket. Also can include remote invocation.  
**Roles and Responsibilities:** Client-Server relation where clients send requests to servers and receive responses. Servers can be clients to other servers. Peer-to-Peer where all nodes in a system are equivalent.  
**Placement:** How to map identified elements to underlying physical infrastructure, whilst optimising performance, reliability, and security. Accounts for communication patterns, reliability, workload, and quality of communication channels. Caching servers can be placed close to clients to reduce latency, but comes with having to update the caching server. Web proxy servers can also be used to access web servers through a firewall.

### Architectural Patterns
**Layering (Logical Organisation):** Vertical organisation of services into service layers, which allows for software abstraction of the system. Hardware/software layer is the platform, and middleware allows applications and services to interact with the platform in an abstracted way.  
**Tiering:** Organises functionality of a given layer into servers. E.g. tiering a client-server application to have clients, application server, and data server.

## Fundamental Models
All distributed systems communicate over a network, of which the latency can affect performance. Nodes can fail, so need systems in place to handle failure to ensure reliability. They're commonly distributed over distance machines over the Internet, which means a large cyber attack surface, so security needs to be handled.

### Interaction Models
**Conventional Algorithms:** Behaviour and state dtermined by sequential steps of a single executing process.  
**Distributed Algorithms:** Behaviour and state determined by intertwining sequencial steps of each involved process, and includes message transmission. Processes proceed and send/receive messages at different rates. There is no global notion of system state, only a global system state, a summation of all local states. Performance of communication measured in latency, bandwidth, and jitter. No global notion of time either due to clock drift.  
**Synchronous Distributed Systems:** Known upper and lower bounds on processing time, network delays, clock drift. Easy to design, used to validate a new distributed algorithm.  
**Asynchronous Distributed Systems:** No bound on time limits. Hard to design, but models the Internet and real systems better. Any system that is valid for asynch systems is also valid for synch ones.

### Failure Models
**Process Omission Failures:** Events where the process has halted and will not execute any further steps of its program. Synchronous model can handle via timeouts, nodes can detect the failure - fail-stop failure. Asynchronous model struggle to deal with this, other nodes cannot reliable detect the failure - crash failure.  
**Channel Omission Failures:** Occurs when either a message is lost on sending or receiving, e.g. network congestion, hardware failure, software errors.  
**Arbitrary (Byzantine) Failures:** Unexpected failures where nodes behave unpredictably or maliciously.  
**Timing Failures:** Occurs in synchronous systems, where nodes may be victim to clock drift.  
**Masking:** Hide or mitigate failures by handling them without notifying the upper layers.  
**Conversion:** Convert failures to a different type of failure and handle it.

### Security Models
**Goals:** Protecting data, processes, and communication, by mitigating incoming attacks, that are threats to processes (identity spoofing, malicious code), communication channels (copy/alter/inject messages), and DoS.  
**Techniques:** Harnessing cryptography to encrypt exchanged and stored data. Authenticate messages based on crytography and digital signatures, and authenticating sender and receiver. Using secure channels that only authenticated parties can use.

## Timing
**Quartz:** Piezoelectric material that produces an electrical charge when it resonates.  
**Atomic Clocks:** Uses the resonant frequency of atoms. Caesium 133 atom used to define time. Used in conjunction with quartz to control a radio wave transmitter tuned to same frequency as the atom, causing it to resonate and be in a higher energy state. Feedback loop: if quartz slows down a detector sends electricity to quartz to increase its frequency.

### Physical Clock Algorithms
**Cristian's Algorithm:** Client sends request to server asking for time. Server replies with its current time. Client accounts for the RTT (round-trip time) to set its own clock from the response. Assumes symmetric network delays, which may not be true.  
**Berkeley Algorithm:** A leader is elected, then periodically polls all nodes for time with Cristian's algorithm. Average time difference is calculated, and the leader's clock is adjusted. The new time is broadcast over the network.  
**Network Time Protocol Algorithm:** Designed to run across the internet. Uses a Stratum model that indicates the device's distance to the reference clock. 0 = high precision clock e.g. atomic clock. NTP client initialises time sync request to nearby NTP server. Client timestamps its send, server timestamps receive and send, client timestamps receive. These 4 numbers are used to estimate network delay and clock offset. Client then adjusts its clock by this offset.

### Event Ordering
**Happened-Before Relation:** Used to define the ordering of events. `a -> b` (`a` happens before `b`) iff:

	a happens before b at the same node in the local order, or 
    a is sending some message m, and b is recipent of message m, or 
    exists event c s.t. a -> c and c -> b. 

If neither `a -> b` nor `b -> a`, a and b are concurrent `a || b`.

**Logical Clocks:** Count the number of events that occur with no relationship to physical time.  
**Lamport Timestamps:** Every node has its own local variable t. On any even occurring at that node, ++t. Message contains value and t. Recipient moves it clock forwards to the timestamp if possible, then increments. If `a -> b` then `L(a) < L(b)`. `L(a) < L(b)` does not imply `a -> b` because L(a) could equal L(b).  
**Vector Clocks:** Able to detect concurrent events. For a system of N nodes, each node has a N-sized vector. When a node executes an event, it increments itself. When a node sends a message, it increments itself, and includes the vector. When a node receives a message, it increments its own counter, and updates the vector by taking the maximum of the two vectors for each counter. `a -> b` if all counters at `a` are smaller or equal to `b`, and are not identical. `a = b` if every counter is identical between `a` and `b`. $a \vert\vert b$ iff $\lnot (a \le b) \land \lnot (b \le a)$.

## Distributed Mutual Exclusion
- Mutual Exclusion: Concurrency control for process synchronisation, where only 1 process can access the critical section (code/data that needs to be accessed atomically) at any time. Requirements: No deadlock, no starvation, fairness, fault tolerance.
- Distributed Systems: No shared memory/clock, and incomplete state of system.
- Properties: Safety (Mutual Exclusion), Liveness (Every node trying to enter must eventually succeed), Fairness (Access to CR should happen in fair order), Performance (Minimise # of messages to enter/exit, Minimise client delay when entering/exiting, Minimise sync delay between exiting CR and next node entering).

### Token Based Algorithms
- If a node posses the unique shared token, it is allowed to enter the critical section. Uses sequence numbers to order requests, and to identify old and current requests.
- Centralised Algorithm: Nodes request for the token, if not available they get added to a queue. Questionable performance - 2 messages enter overhead, entry delay and sync delay (message round trip), single point of failure, server can bottleneck, server needs to be known.
- Ring Token Algorithm: Round robin token system where each node does 1 action before passing it on. Unfair because order is based on the node's location and. Overhead from passing token around. Suspectible to the token-bearing node failing.
- Raymond's Algorithm: Tree based algorithm where children send requests to its parents, and each node keeps these requests in a queue. This propagates to the node with the token, and is moved to the top most node in the queue. Suspectible to starvation, because it is greedy and a node can execute the critical section on receiving the token when it is not its turn to execute.

### Non-Token Based Algorithms
- Nodes communicate with each other to determine who executes critical section. Uses timestamps to determine order.
- Lamport's Algorithm: Each node maintains a request queue and timestamp value. When it wants to execute the critical section, it sends a request to all nodes alongside its own timestamp. Recipient compares timestamp in message to its own timestamp and places request in its own queue. The node grants itself permission to execute critical section if its own request is at the front of the queue and received requests from all other nodes with a higher timestamp. After exiting it adjusts the local queue and sends a release message to all other processes.
- Ricart-Agrawala Algorithm: Improved Lamport's Algorithm. Every node has timestamp and request queue. When a node wants to execute critical section, it sends a reuqest with node id and timestamp to all nodes. Recipient replies iff (recipient is currently not interested in critical section || recipient has lower priority (timestamp)), else it is deferred until it has finished executing critical section. Sender node only executes critical section after it receives all replies, and sends deferred replies upon exiting.

### Quorum Based Algorithms
- Instead of requesting permission from all nodes, it only requests from a subset of sites (quorum). Any two subsets of Quorum contains a common site, which is responsible for mutex.
- Maekawa's Algorithm: Each node maintains a request queue, boolean voted, state wanted/released/held. Subsets of size K, each process in M subsets, where K ~ root(N), M = K, N = # of nodes.
- A requesting node i sends a REQUEST(i, timestamp) to all nodes in its quorum set.
- Recipient j of REQUEST(i, timestamp): If there's no outstanding GRANT, replies with GRANT(j). If outstanding GRANT has higher priority than request, replies with FAILED(j), and queues the request. Else, sends INQUIRE(j) to node which has the outstanding GRANT message.
- Recipient k of INQUIRE(j): Replies with YIELD(k) to j iff k has received a FAILED message from some other site, or k has sent a YIELD to some other site but not received a new GRANT.
- Recipient j of YIELD(k): Send a GRANT(j) to request on top of its request queue. Place k into its request queue.
- Recipient j of RELEASE(i): Delete i from queue, send a GRANT(j) to request on top of its queue.
- i executes critical section on receiving GRANT from all nodes in quorum, on exit sends RELEASE(i) to all nodes in quorum. Vulnerable to deadlock.

## Leader Elections
- Use Cases: Similar nodes and no clear permanent leader. Complex tasks that require close collaboration. Consistency requirement given many distributed writes.
- Advantages: Easier to design and manage, more efficient because its less communication, improved performance due to 1 stream of data.
- Disadvantages: Overhead from passing messages during election. Temporary unavailability during election.  
**Termination:** Election should finish within finite time once leader is selected.  
**Uniqueness:** There is exactly 1 node which considers itself leader.  
**Agreement:** All nodes should know who the leader is.  
**Correctness - Safety:** 1 node is leader and everyone knows this leader's identifier.  
**Correctness - Liveness:** All nodes participate and eventually are elected/receive leader's identifier.  
**Performance:** Minimise messages sent and turnaround time.

#### Ring Leader Election: Elects non-failed node with highest identifier.

1. Nodes are initialised as non-participants.
2. When an election is deemed necessary by a node, marks itself as participant and sends an `election` message with id to left neighbour.
3. When node receives `election` message, compares identifier with its own. If greater than self, forwards message. If it is non-participant, change identifier to itself and forward message, changing self to participant. If participant is itself, declares as leader by sending `elected` message left, marking itself non-participant.
4. Any node that receives `elected` forwards message and marks self as non-participant.

#### Bully Election: Elects non-failed node with highest process ID.  
1. A node `p` triggers an election if it recovers from failure, or detects current coordinator has failed.
2. Broadcasts `ELECTION` message to nodes with higher process id. If none exist, broadcasts `COORDINATOR` message to all nodes, and becomes new coordinator.
3. If no reply to `ELECTION` is received, broadcasts `COORDINATOR` message to all nodes and becomes coordinator.
4. If `ANSWER` is received, waits for `COORDINATOR` message. If not received, starts election again.
5. If a node recieves `ELECTION` from a process with lower ID, replies with `ANSWER`, triggers an election.
6. If `p` receives `COORDINATOR`, treats sender as coordinator.

## Multicast Models - 1-N or M-N
- Group communication where if 1 node sends a message, all nodes in the group delivers it. The group can be either of static or dynamic size, if a node fails the rest of the nodes can continue communicating, and is asynchronous.
- Can be implemented in the network layer (UDP, local network and handling of broadcast/multicast packets, characterised by small packet size, unreliable delivery, out of order delivery) or the application layer (Multicast-style communication built on top of traditional IP networking, available across a range of devices e.g. servers, vms, phones, VPN connected devices).
- Receiving and Delivering: Apps use the multicast operation to send messages and the deliver operation to asynchronously receive a message. After the multicast algorithm receives the message from the network, it may buffer or queue it before delivering to the application. The multicast algorithm uses send and receive methods to send messages between nodes on the network.  
**Basic Multicast:** 1-N where only group members receive messages sent to the group. Groups can be closed/open, where only group members can multicast to closed groups. Uses B-multicast, B-deliver, send(p,m), receive(m), where p=process, m=message. On receive(m) call B-deliver(m). Messages include metadata about sender and destination group. Works for both open and closed groups. Works as long as the sender does not crash.  
**Reliable Multicast:** Delivers reliable sequence of messages, tolerates crashes, assumes closed group. Integrity - A correct (non-faulty) node delivers any message at most once. Agreement - If a correct node delivers message m, all other correct nodes will eventually deliver m. Validity - If a correct node multicasts m, it will eventually deliver m.  
**Gossip Protocols:** Every node that receives a message forwards it to 3 other random nodes. High probability to reach all nodes. Useful for multicasting to a large number of nodes.  
**FIFO Order Multicast:** Messages sent by the same node must be delivered in the same order that they were sent. Messages are sent via multicast, receiver checks whether message is of expected sequence number, if not is held in buffer until expected message arrives.  
**Casual Order Multicast:** Ensures messages are delivered in an order consistent with their casual relationships. Messages tagged with process id, message, and dependencies. Receiver adds message to buffer, and only delivered once their casual dependencies are satisfied.  
**Total Order Multicast:** Ensures messages are delivered to all recipients in the same order. Nodes send messages to sequencer. Sequencer sends sequence number + message id to group, to establish total order. Recipient adds message to buffer. When receives order, ensures message id is in buffer & current sequence number matches sequence number in order message, then delivers message, remove from buffer, and update current sequence number. Single point of failure, hard to change leader safely.  
**FIFO-Total Order Multicast:** Combines FIFO & Total Order multicast. Meets requirements for casual multicast.  

## Consensus Protocols

## Data Replication
Allows for fault tolerance and load balancing, but new issue with data consistency.  

**Primary Backup Replication:** Client sends request to primary server, which checks whether it has been executed before. If not, executes request and stores response. Ensures all nodes get the updated state and response, then responds to client.  
**Primary Server Failure Mitigation:** Leader election used to determine primary server, pending requests are resent if client doesn't receive response within timeout.

### CAP Theorem - Consistency, Availability, Partition Tolerance (Pick 2)
- Consistency: Atomic Data Objects. Ensuring there is a total order on all operations s.t. each request looks like it is executing on a single node in order.
- Availability: Ensures every request received by an online node returns a response.
- Partition Tolerance: When a network is partitioned, it cannot communicate with nodes on the other side.
- AP Systems: Data may be temporarily inconsistent when a network partition occurs. It allows the partitions to serve local data, and once the partition is resolved, the system performs conflict resolution to synchronise the data across partitions to restore consistency.
- CP Systems: Forgoes availabily during network partitions. Employs strong consistency techniques e.g. consensus algorithms, distributed transactions, locking mechanisms. Used in scenarios where data integrity/strong consistency is critical, e.g. financial systems and databases.
- CA Systems: Assumes that network partitions do not occur. Usually employed at single site datacenters, but it does not guarantee network partitions do not occur.

### Scalability
- Ability to handle increased workload with adequate performance.
- Ability to handle increased workload by repeatedly applying a cost-effective strategy to extend a system's capacity.
- Scale Assessment: Scale Parameters - External Parameters e.g. # of clients, workload, # of data. Scalability Metrics - Measurements of System Properties e.g. latency to client, throughput, availability %. Scalability Criteria - Expected Scalability Metrics e.g. P99 latency < 10ms (worst latency observed by 99% of requests), Four nines availability (99.99% uptime).
- Scalabiliy Strategies: ROI on adding more servers. Account for reconfiguration downtime, overhead from communiacation between servers, compute what the bottleneck is e.g. server/resource bottleneck.

### Consistency Models - Strict Consistency, Linearizability, Sequential Consistency, Casual Consistency, (Strict) Eventual Consistency
- A contract between an application and data store that describes the rules the application needs to follow to ensure data store works properly.
- Strong Consistency: Used by CP systems. Requires maintaining a global order of updates, where the replicas agree on the order in which updates are applied.
- Weak Consistency: Used by AP systems. No global ordering and focuses on eventual consistency.
- Active Replication: Clients communicate with a replicas. When a client writes to a replica, the update is propagated amongst replicas via total-order multicast.
- Passive Replication: Primary replica acts as a controller to the clients. Rest of the replicas are used as backup. If primary fails, a new leader is elected.
- Strict Consistency: A write to any replica is immediately available to any other replica. Almost impossible in distributed system due to latency between nodes.
- Linearizability: Slightly weaker strict consistency. Individual read/writes are interleaved into a single total order that respects the local ordering of reads/writes. This allows the client to observe a consistent and coherent view of the system. Requires each node to have a perfectly synchronised clock, and bounded message propagation delays. Uses total-order multicast to ensure replicas handle r/w in same time order.
- Sequential Consistency: Weaker Linearizability. Ensures consistency in the execution of operations as if they happened in a sequential order.  Ensures every node in the system follows this order. Read to replica returns local copy, write operation triggers total-order multicast, from which replicas update local copies and return ack. This allows for replicas to be consistent.
- Casual Consistency: Ensures all writes that have a casual relationship must be seen by every node in system the same way. Reads must be consistent with the casual order. Uses a vector clock, write operations multicast to replicas w/ vector clock, replica updates local copy, reads use the local copy, to ensure consistency.
- Eventual Consistency: Eventually all replicas will be in the same state. Needs the system to tolerate replicas being in an inconsistent state. May have to resolve conflicts. Update local store and propagate changes in background. Dealing with conflict - reconcilation needing global decision via consensus, rollback, delegate to application. Allows for fast read/write and dealing with network partitioning. 
- CRDT (Conflict-Free Replicated Data Type): Operation-based CRDT - Broadcast operation on write, broadcast apply on receive. Used in collaborative editors. State-based CRDT - Broadcast full state on write, broadcast merge state on receive, e.g. grow-only counters. Merging counters by taking the pairwise max of two vectors.
- Quorum-Based Protocols: Improves reliability of system by tolerating fault. Clients read from a quorum and write to a write quorum which intersect. R+W>N to ensure intersection. Set R=W=(N+1)/2 to ensure majority. After a write, the replica updates all nodes in the background. Node failure is tolerated up to the online nodes still form a majority.

## Remote Invocation
- Request-Reply Protocols: Usually synchronous communication between client/server. Can be done with UDP to minimise overhead. Fails if a node dies, message gets lost or is out of order. Timeouts are required to handle errors, may trigger a retransmission of request. Duplicate requests need to be handled. Can be discarded to avoid serving same request, if a duplicate message is received after a reply has been sent, should redo operation, but relies on the operation being idempotent. Could maintain history of replies but this has memory overhead. 3 styles: Request, Request-Reply, Request-Reply-Ack. 
- Remote Procedure Call (RPC) - Allows a client to call a subroutine on another machine without knowing it is remote. RPC client calls local stub procedure with same signature as desired procedure call. Stub assembles required parameters and passes to RPC client runtime. Client runtime uses transport layer to comunicate with server process, and the server disassembles the RPC request and calls the subroutine. However, this makes it more vulnerable to failure and has higher latency. Does not support call by reference.
- RPC Call Semantics: AMO (At Most Once) - Ensures RPC request is executed at most once. ALO (At Least Once) - Ensures RPC request is guaranteed to be executed. Client retransmits RPC request if no response is received. EO (Exactly Once) - Ensures RPC request is executed exactly once. Needs to handle potential duplicates and ensure idempotency.
- RPC Interfaces: Specification of procedures offered by server. Separation of interface and implementation, client cannot directly access server variables. Need to account for parameter passing: CBV vs CBR. Usually specified in an Interface Definition Language.
- Remote Method Invocation: Allows a client to call a method on a remote object. Enables OOP, as well as passing objects as parameters and return values. Remote object references are used to identify objects. When a remote object is invoked, it may invoke other methods, or create a new object. This new object would also have to create a new remote object reference.
- RMI Garbage Collection: Needs to be handled if the language does GC. Usually based on counting references, increment if a different process uses it, decrement if it is no longer needed by remote process.
- RMI Exceptions: Exceptions raised by RMI need to be exposed to caller.

## Web Services
- Provides applications-specific client-server interface s.t. clients can access over the Internet.
- Communication: Can either be asynch (time-consuming operations) or synchronous.
- Loose Coupling: Focus on simple and generic interfaces, and opting for asynchronous communication. (loose coupling over time and space).
- SOAP (Simple Object Access Protocol): Synchronous/asynchronous interactions over Internet, defines how to use XML to represent the content of messages. Also defines how messages are exchanged. Specifies how to combine XML and HTTP to enable client-server communication over Internet.
- RESTful Web Services: Uses HTTP methods for CRUD. Stateless to enable better scalability. Uses directory structure-like URIs to simplify the traversal of resources.

## Peer-to-Peer Systems
- Allows for better scalability due to the distribution of computation, storage, networking. Allows for a better distribution of nodes geographically, better failure mitigation due to more nodes. 
- Server Based Indexing (Napster): Used a central server to index nodes to files. Ran into issues with bottlenecking at the server. Maintaining consistency was easy due to the files involved, music does not mutate. Got shut down because of reliance of a central server to coordinate the system.
- Peer-to-Peer (BitTorrent): Chunks are replicated over peers, can download multiple chunks in parallel. Helps reduce load on individual nodes, as well as speeding up downloads. Incentive system to encourage seeding, by choking nodes who aren't uploading enough. Every node acts as a tracker, organised as a Distributed Hash Table. Each node knows their neighbours and a few further nodes, as well as what files each node has. Evolved to using full DHT system where trackers aren't required to achieve decentralisation.

## Distributed Transactions
- A set of related sequential operations that is guaranteed by the server to be atomic in the presence of multiple clients and server crashes, which requires it to either complete successfull or is rolled back. All objects managed by a server remains in a consistent state, and transactinos deal with crash failure of processes and omission failures in commuincation. Byzantine behivour is not handled.
- ACID Properties: 
Atomicity - Ensures all operations in a transaction are executed as a whole or not at all.
Consistency - Ensures a transaction can only bring the system from a consistent state to another consistent state.
Isolation - Ensures concurrent execution of transaction leaves the system in same state as if they were executed sequentially.
Durability - Ensures the effects of a committed transaction persits even in the case of a system failure.
- Crash Recovery: Objects need to be recoverable. In the event of a server crash, the committed transactions must be durable and stored in permanent storage. When a new node replaces the crashed server, it can recover the objects to ensure durability. Uncommitted transactions are aborted, and recovery procedures are performed to restore objects to their last committed values.
- Concurrency: Synchronizing different transactions to ensure isolation. Needs to be serially equivalent/serializable. This means solving race conditions.
Lost Update - Where one transaction's update is overwritten due to another transaction, due to lack of synchronization.
Inconsistent Retrieval - Where one transaction reads shared data whilst other transactions are modifying it.
- Concurrency Controls:
Locking - Used to regulate access to ensure transaction isolation and data consistency. Locks labelled with the transaction identifier set on each object just before it is accessed. Effective but may lead to contention/performance issues.
Optimistic Concurrency Control - Assumes conflicts between concurrent transactions are infrequent. When a transaction reads required data, the data is associated with a timestamp/version number. Validation then happens to detect conflicts, comparison made between recorded timestamps and current ones. Succeeds if all timestamps/versions match, else is rolled back. Transactions then write changes and commit if success, and update the timestamp on the data item to reflect the update.
Timestamp Ordering - When a transaction enters the system, starting timestamp. Compares timestamp with the last writing transaction of object every r/w. If transaction stamp is earlier, implies reading outdated value. System then validates if the r/w conflicts with other concurrent transactions. Commits or aborts.
- Recovery from Aborts: Aborted transactions are ignored and not stored.
- Dirty Read: Phenomenon that occurs when one transaction reads data that has been modified but not committed. Can be handled by delaying commit until parent operation has committed. If parent aborts, all children should recursively abort. However, this is problemsome due to the recursive nature. Can be enhanced by only accessing objects that have been committed.
- Premature Writes: When multiple transactions try to write to same object. If one of those gets aborted, the object can be left in an inconsistent state. Can be solved by restoring to the before image of the writes.
- Coordinator: One of the servers in a distributed transaction system assume the role of a coordinator. It coordinates the transaction's execution across servers to guarantee consistency and atomicity. Coordinator writes to disk, broadcasts to all children, which reply with OK. Once it receives all OKs, it commits write. The nodes cannot cancel after writing OK.
