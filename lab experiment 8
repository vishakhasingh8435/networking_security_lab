#create simulator object
set ns [new Simulator]

#open trace file
set tf [open out.tr w]
$ns trace-all $tf

#open nam file
set nf [open out.nam w]
$ns namtrace-all $nf

#create nodes
set n0 [$ns node]
set n1 [$ns node]

#create link between nodes
$ns duplex-link $n0 $n1 1Mb 10ms DropTail

#create UDP agent
set udp [new Agent/UDP]
set null [new Agent/Null]
$ns attach-agent $n0 $udp
$ns attach-agent $n1 $null

#connect agents
$ns connect $udp $null

#create traffic (CBR)
set cbr [new Application/Traffic/CBR]
$cbr set packetSize_ 500
$cbr set interval_ 0.01
$cbr attach-agent $udp

#start and stop traffic
$ns at 0.5 "$cbr start"
$ns at 4.5 "$cbr stop"

#finish procedure
proc finish {} {
global ns tf nf
$ns flush-trace
close $tf
close $nf
exec nam out.nam &
exit 0
}

#schedule finish
$ns at 5.0 "finish"
$ns run

#run simulation
$ns run
