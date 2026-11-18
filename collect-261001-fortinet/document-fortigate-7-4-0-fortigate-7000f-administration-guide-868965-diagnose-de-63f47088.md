Diagnose debug flow trace for FPM and FIM activity
Diagnose debug flow trace for FPM and FIM activity
The  diagnose debug flow trace output from the FortiGate-7000F primary FIM CLI shows traffic from all FIMs and FPMs. Each line of output begins with the name of the component that produced the output. For example: 
diagnose debug enable
[FPM04]  id=20085 trace_id=6 func=print_pkt_detail line=5777 msg="vd-root:0 received a packet(proto=6, 10.0.2.3:10001->20.0.0.100:80) from HA-LAG0. flag [S], seq 2670272303, ack 0, win 32768"
[FPM03]  id=20085 trace_id=7 func=print_pkt_detail line=5777 msg="vd-root:0 received a packet(proto=6, 10.0.2.3:10002->20.0.0.100:80) from HA-LAG0. flag [S], seq 3193740413, ack 0, win 32768"
[FPM04]  id=20085 trace_id=6 func=init_ip_session_common line=5937 msg="allocate a new session-0000074c"
[FPM04]  id=20085 trace_id=6 func=vf_ip_route_input_common line=2591 msg="find a route: flag=04000000 gw-20.0.0.100 via HA-LAG1"
[FPM04]  id=20085 trace_id=6 func=fw_forward_handler line=755 msg="Allowed by Policy-10000:"
                                            Running FortiGate-7000F diagnose debug flow trace commands from an individual FPM CLI shows traffic processed by that FPM only.
diagnose debug enable 
[FPM03]  id=20085 trace_id=7 func=print_pkt_detail line=5777 msg="vd-root:0 received a packet(proto=6, 10.0.2.3:10002->20.0.0.100:80) from HA-LAG0. flag [S], seq 3193740413, ack 0, win 32768"
[FPM03]  id=20085 trace_id=7 func=init_ip_session_common line=5937 msg="allocate a new session-000007b2"
[FPM03]  id=20085 trace_id=7 func=vf_ip_route_input_common line=2591 msg="find a route: flag=04000000 gw-20.0.0.100 via HA-LAG1"
[FPM03]  id=20085 trace_id=7 func=fw_forward_handler line=755 msg="Allowed by Policy-10000:"