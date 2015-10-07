# VCAR: VoIP Call Anomaly Reporter

"vcar" is a simple tool for Linux/Mac/Unix systems that can detect some anomalies in phone calls. It currently detects the anomalies described below. "Anomalies" are not errors, or violations of the SIP standard. They are just edge conditions that may confuse some SIP software.

# Using vcar

Save vcar to your Linux/Unix/Mac machine, and make it executable. E.g.: "chmod 755 vcar"

Run it, and provide the filename of a libpcap capture file (as generate by tcpdump, wireshark, etc.). Use the "--stats" flag to get some statistics after the file is analyzed.

# Examples:

```
ecg-vcar capturefile.pcap

ecg-vcar --help

ecg-vcar --stats capturefile.pcap
```

# SDP Connection Information Change

This occurs when one of the SIP signaling devices needs to reconnect the media stream that had already been setup. In particular, either the IP address may change or the RTP port may change. This is a completely-legal behavior whose behavior is described in RFC 3264, the SDP Offer-Answer Protocol.

However, some SIP stacks can have problems with this. For example, the Cisco AS5400 with IOS requires a special configuration to support connection info changes.

```
sdp-connection-info-change : Call-ID BW160440578220310634912045@65.91.52.5 sent from 165.91.52.5 to 65.91.52.76 changed SDP from 65.91.52.78,65.91.52.78:25310 to 65.91.52.78:25310
sdp-connection-info-change : Call-ID SD8t4ve01-5a4d5bbb93c08ed6f0e04c26c1153ca2-v3000i1 sent from 165.91.52.5 to 216.143.61.230 changed SDP from 65.91.52.13:25284 to 65.91.52.76:16936
sdp-connection-info-change : Call-ID BW160440578220310634912045@65.91.52.5 sent from 165.91.52.108 to 66.147.212.138 changed SDP from 165.91.52.108,65.91.52.108:22478 to 65.91.52.108:22478
sdp-connection-info-change : Call-ID 1980608226_127321204@4.55.4.163 sent from 65.91.52.5 to 216.143.61.229 changed SDP from 65.91.52.5:5000 to 65.91.52.45:27656
```

# Failure After Provisional Response

This occurs when a non-100 provisional response is returned for a call, and then later the call fails with a response code other than 487 (Request Cancelled). For example, if a caller gets a 183 Session Progress response, and then later gets a 503 Service Unavailability response, then this type of anomaly would be detected.

```
failure-after-progress: Call-ID BW160446861220310695832209@165.91.52.5 started in frame 1298 failed with 500 at time Mar 22, 2010 16:04:48.125397000 in frame 3002  but had an earlier call-progress indicator

failure-after-progress: Call-ID 9801de1-7a723863-495dab90@192.168.1.127 started in frame 6461 failed with 408 at time Mar 22, 2010 16:04:54.435770000 in frame 7110  but had an earlier call-progress indicator

failure-after-progress: Call-ID 9801de1-7a723863-495dab90@192.168.1.127 started in frame 6441 failed with 408 at time Mar 22, 2010 16:04:54.440033000 in frame 7117  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160444023220310-591209096@165.91.52.5 started in frame 493 failed with 403 at time Mar 22, 2010 16:04:58.344961000 in frame 9824  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW1605024002203101090563373@165.91.52.5 started in frame 14520 failed with 604 at time Mar 22, 2010 16:05:07.202326000 in frame 17805  but had an earlier call-progress indicator

failure-after-progress: Call-ID SDmcae901-cfff9694f6b2d8da10094ecf66f53e73-v3000v3 started in frame 14473 failed with 604 at time Mar 22, 2010 16:05:07.204297000 in frame 17808  but had an earlier call-progress indicator

failure-after-progress: Call-ID SDmcae901-cfff9694f6b2d8da10094ecf66f53e73-v3000v3 started in frame 14439 failed with 604 at time Mar 22, 2010 16:05:07.207663000 in frame 17814  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW1605037532203101033155278@165.91.52.5 started in frame 15807 failed with 400 at time Mar 22, 2010 16:05:13.943972000 in frame 22127  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW1605101712203101185672773@165.91.52.5 started in frame 19689 failed with 604 at time Mar 22, 2010 16:05:14.652443000 in frame 22579  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW1605101712203101185672773@165.91.52.5 started in frame 19673 failed with 604 at time Mar 22, 2010 16:05:14.655623000 in frame 22581  but had an earlier call-progress indicator

failure-after-progress: Call-ID SDjs26b01-3900a191371d1e70287561d46daffa31-v3000v3 started in frame 19629 failed with 604 at time Mar 22, 2010 16:05:14.712697000 in frame 22601  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160520087220310-1428069898@165.91.52.5 started in frame 27077 failed with 500 at time Mar 22, 2010 16:05:21.667923000 in frame 28953  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160520087220310-1428069898@165.91.52.5 started in frame 26890 failed with 500 at time Mar 22, 2010 16:05:21.684997000 in frame 28967  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW1605305962203101320566794@165.91.52.5 started in frame 34906 failed with 481 at time Mar 22, 2010 16:05:31.585014000 in frame 36229  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160537368220310327854761@165.91.52.5 started in frame 40805 failed with 603 at time Mar 22, 2010 16:05:38.128355000 in frame 41795  but had an earlier call-progress indicator

failure-after-progress: Call-ID 289301c1-b6a231c7-65d54782@192.168.1.119 started in frame 81006 failed with 408 at time Mar 22, 2010 16:06:33.076189000 in frame 82086  but had an earlier call-progress indicator

failure-after-progress: Call-ID 289301c1-b6a231c7-65d54782@192.168.1.119 started in frame 80997 failed with 408 at time Mar 22, 2010 16:06:33.087011000 in frame 82108  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160638232220310-817170141@165.91.52.5 started in frame 85919 failed with 500 at time Mar 22, 2010 16:06:45.731553000 in frame 90890  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW160638232220310-817170141@165.91.52.5 started in frame 85759 failed with 500 at time Mar 22, 2010 16:06:45.735619000 in frame 90894  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW16064226522031081186661@165.91.52.5 started in frame 88721 failed with 603 at time Mar 22, 2010 16:06:47.947517000 in frame 91988  but had an earlier call-progress indicator

failure-after-progress: Call-ID BW16064226522031081186661@165.91.52.5 started in frame 88620 failed with 603 at time Mar 22, 2010 16:06:47.949441000 in frame 91990  but had an earlier call-progress indicator

failure-after-progress: Call-ID 488bdf4f-64b65284-9e579b31@192.168.12.136 started in frame 93001 failed with 408 at time Mar 22, 2010 16:06:50.365524000 in frame 93562  but had an earlier call-progress indicator

failure-after-progress: Call-ID 488bdf4f-64b65284-9e579b31@192.168.12.136 started in frame 93001 failed with 401 at time Mar 22, 2010 16:06:50.376381000 in frame 93570  but had an earlier call-progress indicator
```

# Final Stats

vcar can output some final statistics about what it say in the file. Use the "--stats" flag to get that output.

```
$ ./ecg-vcar --stats bsprings_2_00056_20101214110341.pcap
failure-after-progress: Call-ID 70066c58-cc347d6d-ef878d32@10.33.14.67 started in frame 25792 failed with 401 at time Dec 14, 2010 11:04:11.291672000 in frame 26294  but had an earlier call-progress indicator
failure-after-progress: Call-ID 70066c58-cc347d6d-ef878d32@10.33.14.67 started in frame 25792 failed with 401 at time Dec 14, 2010 11:04:12.291592000 in frame 27219  but had an earlier call-progress indicator
failure-after-progress: Call-ID BW1104213021412101296948723@165.91.52.5 started in frame 25993 failed with 400 at time Dec 14, 2010 11:04:30.099479000 in frame 43490  but had an earlier call-progress indicator
sdp-connection-info-change : Call-ID 70066c58-cc347d6d-ef878d32@10.33.14.67 sent from 165.91.52.108 to 164.208.90.230 changed SDP from 10.33.2.61:2252 to 65.91.52.108:22398
sdp-connection-info-change : Call-ID 70066c58-cc347d6d-ef878d32@10.33.14.67 sent from 165.91.52.108 to 164.208.90.230 changed SDP from 65.91.52.108:22398 to 10.33.2.69:2222
failure-after-progress: Call-ID aecc1a9-9d77c8e-af84f94b@10.0.90.144 started in frame 105188 failed with 408 at time Dec 14, 2010 11:05:46.814755000 in frame 105821  but had an earlier call-progress indicator
sdp-connection-info-change : Call-ID BW110556457141210979106786@165.91.52.5 sent from 164.208.90.230 to 65.91.52.108 changed SDP from 0.0.0.0:2234 to 10.0.90.144:2234
sdp-connection-info-change : Call-ID BW110556457141210979106786@165.91.52.5 sent from 164.208.90.230 to 65.91.52.108 changed SDP from 10.0.90.144:2234 to 0.0.0.0:2234
failure-after-progress: Call-ID 621713ef-fde05aa4-624514b1@10.0.90.144 started in frame 124474 failed with 408 at time Dec 14, 2010 11:06:22.138389000 in frame 124852  but had an earlier call-progress indicator
failure-after-progress: Call-ID BW110710548141210909643780@165.91.52.5 started in frame 143009 failed with 400 at time Dec 14, 2010 11:07:23.721251000 in frame 158752  but had an earlier call-progress indicator
sdp-connection-info-change : Call-ID BW110730539141210360082158@165.91.52.5 sent from 164.208.90.230 to 65.91.52.108 changed SDP from 0.0.0.0:2264 to 10.0.90.157:2264
failure-after-progress: Call-ID BW1108017171412101619436374@165.91.52.5 started in frame 178095 failed with 400 at time Dec 14, 2010 11:08:07.627004000 in frame 187435  but had an earlier call-progress indicator

Stats:
411 distinct call-legs observed (as counted by unique Call-ID plus From header combinations)
303.863 seconds between earliest call start and latest call start
3 call-legs have SDP connection info change; 0.00729927%
6 call-legs have Failure after progress; 0.0145985%
```

# Capture File Must be Sorted!

vcar assumes that capture files are sorted chronologically. If detects frame timestamps that are moving back in time, it will report a "file-not-sorted-exception".

# Memory Requirements

vcar uses "tshark" and "awk" to analyze the state of calls through the capture. These both can consume a substantial amount of RAM. For example, a 239 MB capture file made up exclusively of SIP capture data required around 100 MB of RAM to process on a Mac OS X 10.6 machine using tshark 1.4.1 (64 bit) and the stock Mac OS X "awk" (version 20070501).

# Revision History

* 0.2 -- Minor updates.
* 0.1 -- Initial release, detecting failure-after-progress and sdp-connection-info-change.

# Open Source

vcar is an open source script. Use it as the basis for your own exciting stateful call analyzers, or send us ideas for other VoIP call anomalies you'd like to detect.