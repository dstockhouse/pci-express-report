## David

1. Identify the entities that communicate with one another using this protocol.
   In most cases there are two entities with one acting as the client and the
   other as the server, or caller and callee. But in some protocols there may be
   three kinds of entities with a third entity acting as a manager, facilitator,
   or go-between.

2. Describe how devices identify themselves on the network, and become known to
   other devices on the network. If initial connection involves a handshake,
   describe the handshake. If identities are given, describe how the identities
   are assured to be unique.

3. Discuss the issue of traffic and congestion., Describe how the network and
   protocol addresses the problem of either too many devices trying to use the
   network at the same time or trying to push too much data through the network.
   Do devices take turns (how do they know?). Do devices tell get told to stop
   or wait? Do devices get told to slow down or speed up? How are control
   signals separated from data?

## Sean
4. Discuss the issues of security. a. Does the protocol address the issue of
   privacy? If so, how? If not, what risks might exist? b. Does the protocol
   protect against malicious interlopers on the network? If so how? If not, how
   might the network be exploited or compromised by someone who could connect a
   malicious device to the network?

   The PCIe protocol does not have features that directly address the issues of
   security. However, given the physical characteristics of the protocol,
   limited wire length for data transmissions and high operating frequencies.
   The hardware is difficult to tap from a distance. Should one want to access
   data being transmitted directly from PCIe communications, they would need
   direct access to the PCIe slots that facilitate communication between two
   devices.

   c. How does the protocol recover after a failure, i.e. after a power outage
   or some other breakdown?

5. Is efficiency a concern? Does the protocol have features to optimize data
   transmission rates? What does it do to control or adjust speeds? What if
   different devices have different speed capabilities?

   Efficiency is a large concern for the PCIe protocol. One of the main reasons
   why you would want to use the PCIe protocol is for fast communication rates.
   The latest version of PCIe (version 5.0 at the time of writing this
   documentation) can obtain a maximum throughput of 63 GB/s using 16 lanes. One
   contributor that allows PCIe to achieve these fast transmission rates is each
   device has its own serial medium connecting to the root complex or host (in
   the scope of a general purpose computer, the root complex or host would be
   the mother board). This typology varies drastically when compared to the PCI
   protocol which has all devices sharing one medium. This significantly limits
   the rate at which data can be transferred since only one device can
   transmit data a time. Images depicting these differences in medium connection
   between the PCI and PCIe protocol are as follows.

   ![Legacy Medium Configuration](./ImageAssets/PCILegacyMediumConnection.PNG)

   ![Express Medium Configuration](./ImageAssets/PCIExpressMediumConnection.PNG)

   In order to ensure that bits are sent and received at the same rate between
   two devices an addition 2 bits are appended for each 8. These added bits
   allow the phase locked loop (an additional peace of hardware) to measure the
   frequency at which signals are transmitted and synchronize devices.

6. How does the protocol address communication failures? What kinds of failures
   are addressed? Suggest two kinds of failures that are not addressed, and
   discuss what could happen to senders and receivers in a failure of that kind.

   The PCIe protocol accounts for transmission failures at two layers. The
   physical layer, which was previously discussed by adding two bits to 8 bit
   messages to ensure that both devices remain operating at the same
   frequencies. The data link layer, which ensures that the stream of bits that
   were sent, were the same as the ones received. This is achieved via Cyclic
   Redundancy Checks (CRC). **In the case that the CRC identifies an error, a
   request will be sent to the sending device to resend those packets.**

   One unaccounted possibility for error is in the transaction layer (the
   transaction layer is one level of abstraction above the data link layer and
   is specific to the PCIe protocol). The transaction layer has its own header
   and is as follows.

   ![PCIe TLP header](./ImageAssets/PCIE_TLP_memory_write.gif)

   The purpose of the Transaction Layer Packet (TLP) header is to interpret read
   and write instructions. Once the device has fulfilled the request of the read
   or write instruction, a new packet is sent back to the device that initiated
   the request. The PCIe protocol is unclear as to what steps are taken when
   more than one device share the same Requestor ID.