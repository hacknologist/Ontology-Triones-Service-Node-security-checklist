Ontology Triones Service Node security checklist (Ontology Beidou Consensus Cluster Security Implementation Guide)
by SlowMist Security Team & Joinsec Team

content
Architecture Core Goals
Main problems faced
Architecture Core Design
core defense
Recommended overall architecture
Other architectures
1. Single link scheme
2. Multi-load scheme
3. Entry-level plan
Security Hardening Solution
1. RPC/API Security
1.1 Block RPC/API
1.2 RPC/API Protection
2. Command Security
2.1 Password Security
2.2 Enable logging
2.3 Set the minimum transaction price
2.4 Optimization of the number of concurrent connections
2.5 Non-root startup ontology
3. Cybersecurity
3.1 Network Architecture
3.2 Cloud Service Providers
3.3 DDoS Defense
4. Host Security
5. Threat Intelligence
6. NormalNode (normal node) core security configuration summary
Thanks
Architecture Core Goals
Protect the normal communication and operation of the block server
Enhance the overall anti-attack capability of the initial main network
Secure Node Security
Main problems faced
DDoS the initial state mainnet
Unauthorized access and abuse of RPC/API functionality
Communication failure
Architecture Core Design
TSN server isolation
Multi-hop nodes (traffic forwarding on small nodes, high protection on large nodes)
Multi-link high availability
Compared with the EOS architecture, appropriately reduce the cost of a single node, increase the overall number, and ensure the stability of TSN
core defense
RPC/API is turned off by default. When it must be opened, obfuscate the port and set up high defense and other protections
TSN Communication Multilink Design
The TSN server is not exposed on the public network, and communicates through the springboard server (the number of springboard servers should be large)
When the springboard server published on the external network encounters continuous attacks, it communicates through the backup link
To prevent the whole network from scanning and locating the server after the high defense, modify the synchronization port 20338 (similarly 20334-20337 of RPC/API) to the port 80, 443 or 22 of the maximum number of survival in the whole network, which can effectively increase the attacker's location cost.
Recommended overall architecture
Architecture

Architecture Description:

In order to deal with possible DDoS attacks, nodes should prepare multiple links. After the attack, they can communicate through backup links at any time to ensure the smooth start of the main network and continuous block generation.

In the absence of an attack, peripheral nodes communicate through publicly announced nodes.

Other architectures
With the increase of TSN nodes on the main network, the overall anti-attack capability is enhanced, and the newly added nodes can be appropriately downgraded while considering cost savings

1. Single link scheme
Architecture

2. Multi-load scheme
Architecture

3. Entry-level plan
Architecture

Security Hardening Solution
1. RPC/API Security
1.1 Block RPC/API
If unnecessary, it is recommended to prohibit external access to RPC/API.

1.2 RPC/API Protection
It is not recommended to directly expose the RPC/API port of the node to the public network. If there is a need for exposure, please add protection measures before the port

The full node where the RPC/API for query is located is completely isolated from TSN and a defense is set up to ensure that attacks on RPC/API from the external network cannot affect TSN.

2. Command execution security
For detailed instructions on using commands, please refer to the official manual Ontology CLI User Guide

2.1 Password Security
--password, -p

The password parameter is used to specify the account password for Ontology node startup. Because the account password entered on the command line will be saved in the system log, which may easily lead to password leakage, it is recommended not to use this parameter in the production environment.

2.2 Enable logging
--loglevel

The loglevel parameter is used to set the log level of Ontology output. The default value is 1, that is, only logs of the info level and above are output, and the default configuration is used for construction.

2.3 Set the minimum transaction price
--gasprice

The gasprice parameter is used to set the minimum gasprice of the current node's transaction pool to accept transactions. The default value is 0. It is recommended to set a value greater than zero to prevent transaction blocking attacks.

2.4 Optimization of the number of concurrent connections
ulimitThe number of concurrent connections on the P2P port is unlimited, and system parameters and kernel parameters can be optimized to enhance the ability to withstand malicious connection attacks.

2.5 Non-root startup ontology
It is recommended that after the compilation is completed, create a common user account and use this account to start ./ontology, avoid using root, and reduce risks.

2.6 Listen to random ports
--nodeport=PORT1

--rpcport=PORT2

It is recommended to set it to a non-default port. If it is an external service, it is recommended to use the configuration method in Host Security

3. Cybersecurity
3.1 Network Architecture
In order to deal with the problem that the main network of the node is blocked due to possible DDoS attacks, it is recommended to configure a backup network in advance, such as a private VPN network.

3.2 Cloud Service Providers
Tested by the SlowMist security team, Google Cloud, AWS and UCloud have better anti-DDoS attack performance, and the service provider will not temporarily block the server after the DDoS attack, which can restore network access extremely quickly. It is recommended to use the Beidou consensus cluster. . (Please choose a cloud service provider carefully, many cloud service providers will directly shut down the server when they encounter DDoS and other attacks)

3.3 DDoS Defense
In order to deal with possible DDoS attacks, it is recommended that the Beidou consensus cluster be configured with DDoS high-defense services such as Cloudflare and AWS Shield in advance.

4. Host Security
To prevent the whole network from scanning and locating the server after high defense, modify the synchronization port 20338 (similarly 20334/20335/20336 of RPC/API) to the port 80, 443 or 22 of the maximum number of survival in the whole network, which can effectively improve the attacker's positioning cost.
Close unrelated other service ports and customize strict security rules on AWS or Google Cloud.
Change the default 22 port of SSH, configure SSH to only allow login with key (and encrypt the key), prohibit password login, and restrict the IP that accesses the SSH port to only our operation and maintenance IP.
In the case of sufficient budget, it is recommended to deploy excellent HIDS (or it is strongly recommended to refer to the open source OSSEC related practices) to deal with server intrusion in time.
5. Threat Intelligence
It is strongly recommended to do a good job in the collection, storage and analysis of relevant important logs. These logs include: complete communication logs of RPC/API and P2P ports, system logs of hosts, and running logs of node-related programs. For storage and analysis work, you can choose to build an open source solution like ELK (ElasticSearch, Logstash, Kibana), or you can buy an excellent commercial platform.
If you use a mature cloud service provider, there are many threat intelligence-related modules in their console that can be used as a reference to detect abnormalities in time.
When there is a major vulnerability or related attack intelligence on the node, the emergency plan, including disaster recovery strategy and upgrade strategy, will be activated immediately.
Whether there is information exchange in the community.
6. Summary of core security configuration
Select reliable cloud service providers such as Google Cloud, AWS and UCloud as node infrastructure, and the server configuration is above dual-core 4G
The configuration rules prohibit the opening of other service ports that are not related to the node service (SSH only enables certificate login and restricts access to the IP of the SSH port only for our operation and maintenance IP)
Use non-root to run the node program, randomly specify the P2P port at startup, and do not open RPC/API if it is not necessary
Thanks
thank you very much

Ontology Team
Strong support for node security testing has accumulated valuable data for community security.
