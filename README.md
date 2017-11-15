

## Connecting the Disconnected...

Connecting geographic regions that have faced natural disasters or otherwise adverse circumstances continually plagues developed countries, like Puerto Rico after Hurricane Maria. Existing solutions attempt to reconnect regions via store-and-forward systems, while others attempt to drop portable networking devices into a region from above. However, these systems scale poorly and fail to connect broad regions that are isolated from the broader Internet without massive infrastructure overhead.

**SARATOGA** aims to connect large, geographically distant and unconnected areas to the current Internet infrastructure through the use of long-distance radio network bridges combined with a protocol based around data compression, caching, and forward error correction. By placing radio endpoints at an area that is well-connected to the Internet and at another disconnected area, the endpoint at the disconnected area can relay information to the other, which will pass it to the Internet at large.

### Internet for Everyone

Since SARATOGA supports conventional network communications, it services general internet users with typical client devices, like laptops and mobile phones. SARATOGA is designed to benefit users with minimal existing connectivity without the overhead of fiber/wire infrastructure. For example, immediately following a natural disaster, emergency services will be able to set up these nodes rapidly, allowing users with devices to connect and communicate. As the reconstruction of communications infrastructure is costly and time consuming, SARATOGA aims to provide service in the interval in which users can securely, through encryption, make full use of the Internet.

SARATOGA seeks to fulfill users’ connectivity needs by being portable, simple, and cost-effective. Nodes have low power requirements and can be easily transported. These nodes can also be distributed in adverse environments or generally unconnected areas to provide Internet-connectivity. Stronger nodes can be established where there is more power supply with multiple low-cost nodes scattered in a network to connect isolated groups. The use of compression and caching maximizes bandwidth and throughput allowing service to the maximum number of users possible.

### Communities First

SARATOGA is intended for any environment that is (1) disconnected from the Internet and (2) does not currently have the resources or ability to connect directly to the public Internet. Using long-range radio coupled with caching, channel multiplexing, compression, and forward error correction, SARATOGA can supply connectivity to any region with the ability to power a small router-like device with the ability to send out signals similar to HAM radio. Since these devices can be powered via solar power or generators, endpoints can be setup in disconnected locations and provide a way to connect back to the Internet at the connected endpoint, while providing the reliability needed to transmit messages without data loss. Limitations do exist, however. Long-range radio becomes unfeasible without massive amounts of power at the connected endpoint when attempting to send more than several hundred kilometers; therefore, SARATOGA is limited to providing connectivity to regions several hundred kilometers away.

---

## Technical Implementation

SARATOGA uses two long-distance radios to establish a network connection between users in a disconnected area and an endpoint in a connected area that will relay traffic to the high-speed Internet available in modernized regions. All amateur radio endpoints have two interfaces -- one for sending/receiving conventional network data (802.11 WiFi, Ethernet) and another for sending/receiving data on frequencies used for long-distance transmissions. The endpoints will translate data from the conventional network interfaces into data on long-distance radio frequencies and send that data to each other, allowing geographically distant areas to communicate without the infrastructure overhead of large transcontinental cables. 

The connected area endpoint will operate as an Internet service provider for the disconnected area(s). When the endpoint in the connected area receives data over the long-distance frequency, it will translate this data to conventional network data. The endpoint will route this data and send the response back across the long-distance radio bridge to the disconnected area. The disconnected area endpoint will act as a typical router/switch on a computer network -- it will accept connections from individual devices and translate and send their data over the long-distance radio frequency to the corresponding connected endpoint. A working prototype will ideally support point-to-point communication over 10 kilometers at upwards of 1 megabits per second.

There are several hurdles to constructing a network bridge over amateur radio frequencies:
    - Bandwidth, Distance, and Power Trade-Off
    - Low Singal to Noise Ratio
    - High Latency
    - Regulations on Radio Freuqnecies and Transmissions

Each of the above is discussed in the following sections.

### Bandwidth, Throughput, and Distance

To alleviate the bandwidth restrictions and noise in radio transmissions, the project implements a network protocol on top of this infrastructure that uses data compression, forward error correction, and caching. When an endpoint sends data over the long-distance frequency, it will first compress the data using Gzip's data compression algorithm. Other common techniques for reducing traffic on the network bridge also scale well with the proposed protocol. Common network traffic can be cached at the disconnected endpoints in order to reduce the traffic on the radio bridge. Additionally, different connections can be multiplexed across different channels, increasing the throughput of an individual bridge. Directional and omni-directional antennas on the endpoints can also reduce the overlap of transmissions sent into different regions while increasing the power/sensitivity of senders/receivers can improve the distance over transmissions.

### Low Signal to Noise Ratio

To counteract the noise in long-distance transmissions, the compressed data will be encoded with a turbo code, a form of forward error correction which can be used to correct data errors even if they affect relatively large amounts of the data. This will reduce retransmission requests on protocols like TCP, which could further encumber the radio bridge. Compression and strong forward error correction provide the most efficient transmission of information, when information must be retrieved from outside the disconnected area.

### High Latency

Since SARATOGA operates over long distances through radio waves, there is an inherent latency overhead since the transmissions must travel upwards of 10 kilometers. For static web content, this is a non-issue since the resuest and response are sent only once at a time, typically in lock-step. Dynamic content, however, requires a persistent connection. The difficulty therein lies with network protocols like TCP that have a built-in timeout which will close a network connection if a response is not received within a given time window. To counteract these problems, SARATOGA's protocol uses aggressive TCP acknowledgements at the radio bridge endpoints. When a radio bridge endpoint receives a TCP segment, it will automatically send an acknowledgement in order to keep the connection alive and foward the data to the recipient. If the recipient does not send an acknowledgement back to the endpoint, then the packet is assumed lost and the connection subsequently closed. Otherwise, the acknowldgement is simply discarded.

### Regulations on Radio Frequencies and Transmissions

Some countries place restrictions on the type of data that can be transmitted over certain radio frequencies. The United States of America's Federal Communication Commision (FCC), for instance, restricts the use of encryption on some amateur radio bands as part of Title 47. There, however, exist radio frequencies where this ruling does not apply that can be used for point-to-point ecrypted transmissions. Some alternatives include WiFi over Long-Distance (WiLD) and the ISM  (Industrial, Scientific, and Medical) band for long-distance encrypted radio transmissions. For countries without such regulations, more optimal frequencies can be used.

### Status of Current Work

Currently, SARATOGA is focusing on the implementation of the network protocol to support efficient transmission of data. At this time, the project has no hardware prototypes but has software support for forward error correction of network data via open-source libraries. Currently the work focuses on how the project differs from its predecessors, specifically in the inclusion of compression, forward error correction, and caching. Previous projects using long-distance radio to facilitate Internet connectivity in off-the-grid locations have had moderate success, which can be furthered by the changes we propose.

---

## Unique Advantages

### Portability 

A standard SARATOGA node can be incredibly lightweight, power efficient, and portable.  Each Linksys router is compact and weighs under a pound.  As the power demands are light for the router, relatively small batteries, under two pounds,  can be used to power the endpoint for up to ten hours. Therefore, it is reasonable to assume that a fully functioning node could weigh under 5 pounds and would be very easy to place in any location regardless of the transportation difficulties.  Naturally, more powerful endpoints would require significantly added weight requirements due to amplifiers, batteries, solar panels, and antennas.  However, the added equipment could be easily distributed among a few individuals and transported with ease.  

### Power

The home routers used in SARATOGA are expected to consume on the order of a few watts of power. For example, the Linksys WRT5G data sheet specifies 0.5 A of current at 12 V, or 6 watts according to [http://downloads.linksys.com/downloads/datasheet/wrt54gv8-ds.pdf](LinkSys).  Other comparable home routers are expected to have a similar power draw. This is the minimum required to run a small, short-range node; however, depending on the required range and bandwidth additional power may be needed to run an amplifier. Amplifiers used in amateur radio use power measured in the hundreds of watts. 

A small node could therefore function for less than 10 watts, while a much larger node could draw up to 1000 watts. As with hardware costs, the exact power usage will vary significantly depending on the exact choice of hardware for the node. The hardware will depend on the deployment conditions, and again, this allows the user to readily scale power consumption with the range desired in a particular deployment scenario.

### Applications

An estimate of the bandwidth provided by long-range radios is 1 megabit per second. Without compressing data packets, messengers such as Zangi, WhatsApp, and many other communication applications with low data usage will be able to function at nearly full capacity using the proposed off-the-grid internet. Other than messengers, streaming applications such as Netflix and YouTube can also be used;  however, watching video over a 1Mb/s means that either the video will be low quality (like watching VHS movies) or a large amount of time will be spent buffering the video at high quality.

In order to decrease traffic over the radio bridges, the data is compressed with Gzip's DEFLATE algorithm. Compression effectiveness varies by data, but assume that the algorithm can reduce the size of packets by 60 percent. That means more than twice as much data can be sent and more data can fit into the cache. Due to the high latency of traffic, however, real-time applications will not work well over this network topology. SARATOGA is intended to provide low-overhead connectivity for minimally interactive web function with minimal infrastructure - ideal for post-disaster and unstable environments.


---

## Differentiation

Several past projects have connected disconnected regions with varying amounts of success. The Latin American Networking School Foundation in Venezuela, for example, created a 279 kilometer WiFi connection. The TIER project at the University of California, Berkeley created a similar long-range WiFi network and successfully supported video conferences between patients and doctors that were geographically distant. More recently, Google's Project Loon provided Internet to Puerto Rico after Hurricane Maria.

These projects have had moderate success, which SARATOGA seeks to improve upon with a novel combination of existing technologies alongside a different network topology. Unlike the aforementioned projects, SARATOGA uses a point-to-point bridge between regions. Rather than place the radio nodes far from the users, like in the previous projects, this project keeps the endpoints close the users to minimize latency and error. This results in only one streamlined high-latency, moderately error-prone connection across the two regions. This project should support higher bandwidth operations than previous projects as well by compressing and caching data and reduce re-transmission with forward error correction.

## Affordability

SARATOGA aims to re-purpose existing wireless communication devices for longer-range communication, in combination with software techniques to overcome existing limitations. Therefore, SARATOGA uses only low-cost, off-the-shelf hardware, which minimizes the cost for a single node. A single node requires, at a minimum, only a consumer router capable of running the necessary software such as the Linksys WRT5GL, which is available online for about 50 USD as of November 2017 shown here on [https://www.amazon.com/Linksys-WRT54GL-Wireless-G-Broadband-Router/dp/B000BTL0OA](Amazon). This router can run open-source firmware like OpenWRT, which supports modification of network protocols. Antennae can substantially increase range and cost between 100 USD up to thousands of dollars; using a small antenna, we estimate ranges of up to 10 miles with good line of sight. Amplifiers increase range at an additional hardware cost, and amateur radio amplifiers also come at a wide range of power levels and cost, from the low hundreds of dollars into the thousands. In total, we estimate that a minimal functioning node could be built for less than a 100 USD, while an ultra-long-range node could cost several thousand dollars.

## Social Impact

This project provides a hot-plug solution to Internet connectivity in disconnected communities. First, the simplicity of being able to plug in two devices and have the system work makes the solution approachable, even by people who are not technically literate. This means that even extremely small communities can gain connectivity by setting up a few nodes themselves without having to pay expensive network technicians. Second, application layer programs using the network will see almost no change in network behavior since the endpoints communicate with client devices with conventional network protocols.

The SARATOGA nodes are low-power and can run on low-cost commercial-off-the-shelf hardware, which is ideal for growing communities that do not have the stability or money to bring fiber/wire infrastructure to their location. Moreover, these nodes can be routed into a mesh network for increased redundancy of cached data or multiplexed for increased throughput. All of these features make this solution optimal for disaster struck areas, or simply growing communities, to quickly connect to the Internet and scale their connections as they grow until they can attain a more stable and efficient hardware solution.

## Scalability

Since SARATOGA only needs to alter conventional network traffic at one point during the transmission process, it scales with current network devices. The endpoints support 802.11 and Ethernet connections with clients, so devices available in connected regions operate without any issues. Included caching, data compression, and forward error correction reduce the bandwidth constraints on the long-distance radio connection between disconnected/connected regions, so links should support moderate amounts of users for each disconnected location. The support for normal devices (i.e. phones/laptops), allows users in the disconnected region to join the connected world with existing technology. This generality allows transferability of new developments between these regions and allow the disconnected regions to develop technologically alongside the connected world. Moreover, additional channels/substations can be added to the network topology to support more concurrent users as the community grows. If the individual P2P links become overwhelmed, the wireless nature of the topology allows transitioning to other topologies, like a mesh network, for congestion-aware routing. In an extreme case, a dense enough mesh of this protocol could implement a high-latency border-gateway protocol (BGP), much like the modern Internet.

---

### Our Team

- **Parker Diamond**
    - **Full Name:** Joseph Parker Diamond
    - **Email Address:** jparkerdiamond@gmail.com
    - **Team Role:** Co-Lead, Software Lead
    - **Bio:** Joseph Parker Diamond is a Computer Science Master's student at the University of Tennessee, Knoxville (UT). He received his Bachelor's in Computer Science in 2017 from UT with a double major in Mathematics. He is currently conducting research with UT's security lab Volsec on onion routing anonymity in the presence of global adversaries, cryptocurrency pool attacks, and opportunistic networking.
    - **Relevant Background:** https://volsec.eecs.utk.edu/research/

- **Kyle Birkeland**
    - Full Name: Kyle Birkeland
    - Email Address: kylebirkeland@gmail.com
    - Team Role: Co-Lead, Hardware Lead
    - Bio: Kyle Birkeland is an undergraduate Computer Science student at the University of Tennessee, Knoxville.  He is also a network engineer for the University where he focuses on security, high availability, and automation.

- **Kris Brown**
    - Full Name: Kris Allen Brown
    - Email Address: kbrown42@vols.utk.edu
    - Team Role: Social Impact and Community Analyst
    - Bio: Joseph Parker Diamond is Computer Science Master's student at the University of Tennessee, Knoxville (UT). He received his Bachelor's in Political Science in 2012 from Middle Tennessee State University (MTSU). He is currently conducting research at UT in collaboration with the Veteran's Administration on natural language processing for psychiatric medical health records.


- **Joseph Connor**
    - Full Name: Richard Joseph Connor
    - Email Address: rconnor6@vols.utk.edu
    - Team Role: Systems Architect
    - Bio: Joseph Connor is a PhD student in computer science at the University of Tennessee, with a concentration in cryptography and network security. He received his B.S. in computer science from UT in 2016.

- **Tyler Marshall**
    - Full Name: Tyler Eugene Marshall
    - Email Address: ty.e.marshall@gmail.com
    - Team Role: Research Support
    - Bio: Tyler Marshall is a Master's candidate in Computer Science at the University of Tennessee , Knoxville. He received his Bachelor's at UT, majoring in computer science with a minor in mathematics. Tyler's background in computer science covers a wide breadth of topics at a relatively shallow level. His background in mathematics is focused on continuous math (ODEs, PDEs, etc). His long term goal after completing his Master's is to work in R\&D in either computer networking or operating systems.

- **Sara Mousavi**
     - Full Name: Sara Mousavi
     - Email Address: mousavi@vols.utk.edu
     - Team Role: Research Support
     - Bio: Sara Mousavi is a computer science PhD student at The University of Tennessee. She has a Bachelor degree in computer engineering. Starting from her PhD, she has been working on accelerating the write speed of erasure-coded data storage systems and improving the adaptivity of erasure codes. She will receive her masters in December 2017 and will continue her PhD in big data analysis and software engineering, for measuring software reliability

- **Jared Smith**
    - Full Name: Jared Michael Smith
    - Email Address: jms@vols.utk.edu 
    - Team Role: Research Lead
    - Bio: Jared M. Smith is a Computer Science PhD student at The University of Tennessee where he works on enhancing the resiliency of the Internet against DDoS attacks and adverse network conditions. Jared has a Bachelor's and Master's in Computer Science, both from UT, where he founded the university's hackathon and cyber security organization. Jared is also currently a Research Scientist at Oak Ridge National Lab where he leads several cyber security projects for the US DHS and US DOE.
