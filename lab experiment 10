#Create simulator
set ns [new Simulator]

#Open trace file
set tf [open ipsec.tr w]
$ns trace-all $tf

set nf [open ipsec.nam w]
$ns namtrace-all $nf

#Finish procedure
proc finish {} {
global ns tf nf
$ns flush-trace
close $tf
close $nf
exec nam ipsec.nam &
exit 0
}

#Create nodes
set n0 [$ns node]  ;#Sender
set n1 [$ns node]  ;#Router
set n2 [$ns node]  ;#Receiver

#Links
$ns duplex-link $n0 $n1 1Mb 10ms DropTail
$ns duplex-link $n1 $n2 1Mb 10ms DropTail

#Queue limit
$ns queue-limit $n1 $n2 20

#Normal traffic
set udp1 [new Agent/UDP]
$ns attach-agent $n0 $udp1

set null1 [new Agent/Null]
$ns attach-agent $n2 $null1

$ns connect $udp1 $null1

set cbr1 [new Application/Traffic/CBR]
$cbr1 set packetSize_ 500
$cbr1 set interval_ 0.02
$cbr1 attach-agent $udp1

#IPSEC-Like Traffic
set udp2 [new Agent/UDP]
$ns attach-agent $n0 $udp2

set null2 [new Agent/Null]
$ns attach-agent $n2 $null2

$ns connect $udp2 $null2

set cbr2 [new Application/Traffic/CBR]
#Increased packet size to simulate IPsec header overhead
$cbr2 set packetSize_ 600
$cbr2 set interval_ 0.02
$cbr2 attach-agent $udp2

#Colors
$ns color 1 Green
$ns color 2 Orange
$udp1 set fid_ 1
$udp2 set fid_ 2

#Schedule Traffic
#Normal traffic runs first
$ns at 1.0 "$cbr1 start"
$ns at 3.0 "$cbr1 stop"

#IPsec-Like traffic runs next
$ns at 3.0 "$cbr2 start"
$ns at 5.0 "$cbr2 stop"

#End simulation
$ns at 5.5 "finish"
$ns run
#End simulation
$ns at 5.5 "finish"
$ns run
