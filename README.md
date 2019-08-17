The following changes have been made to OSPFv2 and OSPFv3 Yang model.

1. Change the following Yang module from ospfv2 to ospf in order to have a common module supporting both OSPFv2 and OSPFv3. The idea is to create OSPF common file supporting the core functionality of both OSPFv2 and OSPFv3. Then use augment to augment the core model into OSPFv2 and OSPFv3. Both OSPFv2 and OSPFv3 are under protocol with the name ospf and ospfv3. Although the model is splitted to core and ospfv2/v3 spcific, the produced OSPFv2 Yang tree maintains high similarity to the original openconfig OSPF Yang tree except fixing the defects on the original tree.

File has been changed to OSPF core.
openconfig-ospfv2-area-interface.yang -> openconfig-ospf-area-interface.yang
openconfig-ospfv2-area.yang           -> openconfig-ospf-area.yang
openconfig-ospfv2-common.yang         -> openconfig-ospf-common.yang
openconfig-ospfv2-global.yang         -> openconfig-ospf-global.yang
openconfig-ospfv2.yang                -> openconfig-ospf.yang

Add OSPFv2 specific files to augment ospf core model to form OSPFv2 model. All MPLS definitions are move to the following OSPFv2 specific files.
- openconfig-ospfv2-area-interface.yang
- openconfig-ospfv2-global.yang
- openconfig-ospfv2-lsdb.yang

Add OSPFv3 specific files to augment ospf core model to form OSPFv3 model.
- openconfig-ospfv3-area-interface.yang 

2. In openconfig-network-instance.yang file
  a) Change openconfig-ospfv2 to openconfig-ospf
  b) Add reference to ospfv3-top container 

3. In openconfig-ospf.yang file
  a) Add ospfv3-top grouping which is referenced by openconfig-network-instance.yang

4. In openconfig-ospf-global.yang file
  a) Add abr-capability under ospf-global-config. Rational behind: the ABR in RFC2328 defines a router to be ABR when it participats multiple OSPF areas and it does not matter whether it connects to backbone area or not. The RFC3509 provides an alternate implementation where the ABR is required to participate backbone area. Cisco supports the latter implementation while the Juniper supports the former. Ciena supports both. We think it is important for an operator to understand the ABR definition used by different vendors to come out a peroper network design.
  b) Change ospfv2 to ospf
  c) In state container of the ospf-global-structural, add ospfv2-global-lsdb-structure which contains OSPFv2 LSA database with the AS flooding domain.  d) Move summary-route-cost-mode and igp-shortcuts to openconfig-ospfv2-global.yang and uses these two parameters only for OSPFv2.

5. In openconfig-ospf-types.yang file
  a) Add OSPF-AREA-TYPE definition including NORMAL, STUB, and NSSA
  b) Add OSPF-ABR-TYPE definition including RFC3509-COMPATIBLE and RFC2328-COMPATIBLE

6. In openconfig-ospf-common.yang file
  a) Change ospfv2 to ospf
  b) Move MPLS related definition to openconfig-ospfv2-global.yang

7. In openconfig-ospf-area.yang file
  a) Change ospfv2 to ospf
  b) Add address-ranges, stub-default-cost and import-summaries under area/config 
  c) Add ospfv2-area-lsdb-structure under area/state for OSPFv2

8. In openconfig-ospf-area-interface.yang

9. In openconfig-ospfv2-lsdb.yang file
  a) Split definition into ospfv2-global-lsdb-structure, ospfv2-area-lsdb-structure and ospfv2-link-lsdb-structure

10. In openconfig-ospf-area-interface.yang
  a) Change ospfv2 to ospf
  b) Add interface-transmission-delay under ospf-area-interface-timers-config
  c) Add reference to ospfv3-area-interface-config which contains OSPFv3 specific configuration
  d) Add default value to hello-interval, dead-interval, retransmission-interval, priority, passive, and metric.

11. Add openconfig-ospfv2-global.yang
  a) This submodule contains the OSPFv2 specific parameters including summary-route-cost-mode and igp-shortcuts.

12. Add openconfig-ospfv2-area-interface.yang
  a) Move mpls container from openconfig-ospf-area-interface.yang
  b) Move ospfv2-area-interface-mpls-config container from openconfig-ospf-area-interface.yang

13. Add openconfig-ospfv3-area-interface.yang
  a) Add instance-id under ospfv3-area-interface-config, which is required by OSPFv3
  b) Add interface-id under ospfv3-area-interface-config, which is a 32-bit numbered uniquely identifying the interface.
  c) Define ospfv3-area-interface-config which contains OSPFv3 specific configuration


To-DO list:
1. The OSPFv2 LSDB: still have definition errors
2. The OSPFv3 LDDB is not defined yet.
