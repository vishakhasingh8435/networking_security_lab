#Create simulator object
set ns [new Simulator]

#Open trace files
set tf [open dos_attack.tr w]
$ns trace-all $tf

set nf [open dos_attack.nam w]
$ns namtrace-all $nf

#Finish procedure
proc finish {} {
global ns tf nf
$ns flush-trace
close $tf
close $nf
exec nam dos_attack.nam &
exit 0
}

#Create nodes
set n0 [$ns node]   ;#Normal Sender
set n1 [$ns node]   ;#Attacker
set n2 [$ns node]   ;#Router
set n3 [$ns node]   ;#Destination

#Create links
$ns duplex-link $n0 $n2 1Mb 10ms DropTail
$ns duplex-link $n1 $n2 1Mb 10ms DropTail
$ns duplex-link $n2 $n3 1Mb 10ms DropTail

#Set queue limit to show congestion clearly
$ns queue-limit $n2 $n3 10

#Normal traffic: UDP + CBR from n0 to n3
set udp0 [new Agent/UDP]
$ns attach-agent $n0 $udp0

set null0 [new Agent/Null]
$ns attach-agent $n3 $null0

$ns connect $udp0 $null0

set cbr0 [new Application/Traffic/CBR]
$cbr0 set packetSize_ 500
$cbr0 set interval_ 0.05
$cbr0 attach-agent $udp0

#Attack traffic: UDP flood from n1 to n3
set udp1 [new Agent/UDP]
$ns attach-agent $n1 $udp1

set null1 [new Agent/Null]
$ns attach-agent $n3 $null1

$ns connect $udp1 $null1

set attack [new Application/Traffic/CBR]
$attack set packetSize_ 1000
$attack set interval_ 0.001
$attack attach-agent $udp1

#Colors for NAM
$ns color 1 Blue
$ns color 2 Red

$udp0 set fid_ 1
$udp0 set fid_ 2

#Schedule events
$ns at 0.5 "$cbr0 start"
$ns at 1.0 "$attack start"
$ns at 4.0 "$attack stop"
$ns at 4.5 "$cbr0 stop"

#End silmulation
$ns at 5.0 "finish" 
$ns run
