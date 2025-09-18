# Packet_5G

### Language | Idioma
**English** | [Português](packet5G.md)

## Program Execution

To execute the program and perform the desired simulations, it's necessary to install the program dependencies:
1. [Network Simulator (Ns3)](https://www.nsnam.org/)
2. [Mmwave Module](https://github.com/nyuwireless-unipd/ns3-mmwave)

It's possible to install the simulator and the respective module separately, or access through a docker https://hub.docker.com/r/fedeit/ns3-mmwave, where the simulator is already configured and ready to use.

#### Installing the Ns3 Simulator with mmWave Module:
In the official repository of the [mmWave](https://github.com/nyuwireless-unipd/ns3-mmwave) module for the Network Simulator, you can find the detailed manual to install the simulator along with mmwave. To perform the installation, the following steps are necessary:

```bash
git clone https://github.com/nyuwireless-unipd/ns3-mmwave.git
cd ns3-mmwave
./ns3 configure --disable-python --enable-examples && ./ns3 build
```

To verify if all modules were installed correctly, with the following command it's possible to run the following example simulation:

```bash
./ns3 run mmwave-simple-epc
```

---
## Simulation Code (Packet5G.cc)

The network simulator uses C++ as the main language, where to compile the simulation it's necessary that the file be built before being executed. For this, it's necessary to run the following code in the prompt:

```bash
./ns3 build Packet5G.cc
```

- Includes Used in the Project
```cpp
  #include "ns3/core-module.h"
  #include "ns3/network-module.h"
  #include "ns3/internet-module.h"
  #include "ns3/applications-module.h"
  #include "ns3/mobility-module.h"
  #include <ns3/buildings-helper.h>
  #include "ns3/config-store.h"
  #include "ns3/mmwave-helper.h"
  #include "ns3/ipv4-global-routing-helper.h"
  #include "ns3/applications-module.h"
  #include "ns3/log.h"
  #include "ns3/isotropic-antenna-model.h"
  #include <map>
  #include "ns3/netanim-module.h"
  #include "ns3/flow-monitor.h"
  #include "ns3/flow-monitor-helper.h"
  #include "ns3/mmwave-point-to-point-epc-helper.h"
  #include "ns3/point-to-point-helper.h"
  #include "ns3/global-route-manager.h"

  using namespace ns3;
  using namespace mmwave;
```

- Functions responsible for monitoring transmitted packets
```cpp
void
TxMacPacketTraceUe (Ptr<OutputStreamWrapper> stream, uint16_t rnti, uint8_t ccId, uint32_t size)
{
  *stream->GetStream () << Simulator::Now ().GetSeconds () << "\t" << (uint32_t)ccId << '\t' << size << std::endl;

}
void
Traces (std::string filePath)
{
  std::string path = "/NodeList/*/DeviceList/*/ComponentCarrierMapUe/*/MmWaveUeMac/TxMacPacketTraceUe";

  filePath = filePath + "TxMacPacketTraceUe.txt";

  AsciiTraceHelper asciiTraceHelper;

  Ptr<OutputStreamWrapper> stream1 = asciiTraceHelper.CreateFileStream (filePath);

  *stream1->GetStream () << "Time" << "\t" << "CC" << '\t' << "Packet size" << std::endl;

  Config::ConnectWithoutContext (path, MakeBoundCallback (&TxMacPacketTraceUe, stream1));

}
```

- Main Function
```cpp
int
main (int argc, char *argv[])
{
  uint16_t numEnb = 1; // Number of eNBs
  uint16_t numUe = 10; // Number of UEs per eNB
  double distance = 500.0; // Distance between eNB and UE
  double simTime = 10.0; // Simulation time
  bool harqEnabled = true;
  bool rlcAmEnabled = true;
  int mcsDl = -1; // Downlink MCS
  int mcsUl = -1; // Uplink MCS
  double totalBandwidth = 20e6; // Total bandwidth in Hz
  double frequency = 28e9; // Carrier frequency in Hz
  uint32_t lambda = 1000; // Traffic generation rate

  CommandLine cmd;
  cmd.AddValue ("numEnb", "Number of eNBs", numEnb);
  cmd.AddValue ("numUe", "Number of UEs per eNB", numUe);
  cmd.AddValue ("distance", "Distance between eNB and UE", distance);
  cmd.AddValue ("simTime", "Total duration of the simulation", simTime);
  cmd.AddValue ("harq", "Enable Hybrid ARQ", harqEnabled);
  cmd.AddValue ("rlcAm", "Enable RLC Acknowledged Mode", rlcAmEnabled);
  cmd.AddValue ("mcsDl", "Downlink MCS", mcsDl);
  cmd.AddValue ("mcsUl", "Uplink MCS", mcsUl);
  cmd.AddValue ("lambda", "Traffic generation rate", lambda);
  cmd.Parse (argc, argv);

  Time::SetResolution (Time::NS);

  // Object Creation

  Ptr<MmWavePhyMacCommon> phyMacConfig0 = CreateObject<MmWavePhyMacCommon> ();
  phyMacConfig0->SetBandwidth (totalBandwidth);
  phyMacConfig0->SetCentreFrequency (frequency);
  Ptr<MmWaveComponentCarrier> cc0 = CreateObject<MmWaveComponentCarrier> ();
  cc0->SetConfigurationParameters (phyMacConfig0);
  cc0->SetAsPrimary (true);
  std::map<uint8_t, MmWaveComponentCarrier> ccMap;
  ccMap [0] = *cc0;

  // Creation and default definition of Helpers
  Config::SetDefault ("ns3::MmWaveHelper::ChannelModel",StringValue ("ns3::ThreeGppSpectrumPropagationLossModel"));// channel model used
  Config::SetDefault ("ns3::ThreeGppChannelModel::UpdatePeriod",TimeValue (MilliSeconds(100)));
  Config::SetDefault ("ns3::MmWaveHelper::PathlossModel",StringValue ("ns3::ThreeGppRMaPropagationLossModel"));
  Config::SetDefault ("ns3::ThreeGppRMaPropagationLossModel::ShadowingEnabled",BooleanValue (false));
  
  Ptr<MmWaveHelper> mmwaveHelper = CreateObject<MmWaveHelper> ();
  mmwaveHelper->SetChannelConditionModelAttribute ("UpdatePeriod", TimeValue (MilliSeconds (0)));
  mmwaveHelper->Initialize();
  mmwaveHelper->SetHarqEnabled (harqEnabled);
  mmwaveHelper->SetSnrTest (true);
  
  Ptr<MmWavePointToPointEpcHelper> epcHelper = CreateObject<MmWavePointToPointEpcHelper> ();
  mmwaveHelper->SetEpcHelper (epcHelper);
  mmwaveHelper->SetCcMap (ccMap);

  // Create nodes
  NodeContainer enbNodes;
  enbNodes.Create (numEnb);
  NodeContainer ueNodes;
  ueNodes.Create (numUe);

  // Install mobility model
  Ptr<ListPositionAllocator> enbPositionAlloc = CreateObject<ListPositionAllocator> ();
  enbPositionAlloc->Add (Vector (250.0, 250.0, 25.0));
  MobilityHelper enbmobility;
  enbmobility.SetMobilityModel ("ns3::ConstantPositionMobilityModel");
  enbmobility.SetPositionAllocator (enbPositionAlloc);
  enbmobility.Install (enbNodes);

  MobilityHelper uemobility;
  uemobility.SetMobilityModel ("ns3::ConstantPositionMobilityModel");
  Ptr<ListPositionAllocator> uePositionAlloc = CreateObject<ListPositionAllocator> ();
  
  // User positions (optimized by Batman algorithm)
  uePositionAlloc->Add (Vector (290.52, 196.50, 1.5));
  uePositionAlloc->Add (Vector (238.58, 315.92, 1.5));
  uePositionAlloc->Add (Vector (299.93, 252.57, 1.5));
  uePositionAlloc->Add (Vector (179.48, 225.16, 1.5));
  uePositionAlloc->Add (Vector (294.31, 226.83, 1.5));
  uePositionAlloc->Add (Vector (193.93, 284.75, 1.5));
  uePositionAlloc->Add (Vector (291.42, 278.00, 1.5));
  uePositionAlloc->Add (Vector (200.28, 244.72, 1.5));
  uePositionAlloc->Add (Vector (248.56, 200.02, 1.5));
  uePositionAlloc->Add (Vector (318.96, 306.54, 1.5));

  uemobility.SetPositionAllocator (uePositionAlloc);
  uemobility.Install (ueNodes);

  // Install mmWave Devices
  NetDeviceContainer enbmmWaveDevs = mmwaveHelper->InstallEnbDevice (enbNodes);
  NetDeviceContainer uemmWaveDevs = mmwaveHelper->InstallUeDevice (ueNodes);

  // Install the IP stack on the UEs
  InternetStackHelper internet;
  internet.Install (ueNodes);
  Ipv4InterfaceContainer ueIpIfaces;
  ueIpIfaces = epcHelper->AssignUeIpv4Address (NetDeviceContainer (uemmWaveDevs));

  // Assign IP address to UEs, and install applications
  for (uint32_t u = 0; u < ueNodes.GetN (); ++u)
    {
      Ptr<Node> ueNode = ueNodes.Get (u);
      // Set the default gateway for the UE
      Ptr<Ipv4StaticRouting> ueStaticRouting = Ipv4RoutingHelper::GetRouting <Ipv4StaticRouting> (ueNode->GetObject<Ipv4> ()->GetRoutingProtocol ());
      ueStaticRouting->SetDefaultRoute (epcHelper->GetUeDefaultGatewayAddress (), 1);
    }

  mmwaveHelper->AttachToClosestEnb (uemmWaveDevs, enbmmWaveDevs);
  mmwaveHelper->EnableTraces ();

  // Install and start applications on UEs and remote host
  uint16_t dlPort = 10000;
  uint16_t ulPort = 20000;
  PacketSinkHelper dlPacketSinkHelper ("ns3::UdpSocketFactory", InetSocketAddress (Ipv4Address::GetAny (), dlPort));
  PacketSinkHelper ulPacketSinkHelper ("ns3::UdpSocketFactory", InetSocketAddress (Ipv4Address::GetAny (), ulPort));
  ApplicationContainer serverApps = dlPacketSinkHelper.Install (ueNodes);
  
  UdpClientHelper dlClient (ueIpIfaces.GetAddress (0), dlPort);
  dlClient.SetAttribute ("MaxPackets", UintegerValue (0xFFFFFFFF));
  dlClient.SetAttribute ("Interval", TimeValue (Time ("0.0001")));
  dlClient.SetAttribute ("PacketSize", UintegerValue (1024));

  ApplicationContainer clientApps = dlClient.Install (epcHelper->GetPgwNode ());
  serverApps.Start (Seconds (0.01));
  clientApps.Start (Seconds (0.01));

  Traces ("/tmp/");

  Simulator::Stop (Seconds (simTime));
  Simulator::Run ();
  Simulator::Destroy ();
  return 0;

}
```

## References