gozebra
===

## About ##

A quagga zapi library implemented in Go.

## Usage ##
```go
cli, _ := zebra.NewClient("unix", "/var/run/quagga/zserv.api", zebra.ROUTE_BGP)

ch, _ := cli.StartRecieving()
go func() {
	for {
		m := <-ch
		log.Debug(m)
	}
}()

// this asks zebra to send all interface information
cli.SendCommand(zebra.INTERFACE_ADD, nil)

b := &zebra.IPv4RouteBody{
	Type:         zebra.ROUTE_BGP,
	SAFI:         zebra.SAFI_UNICAST,
	Message:      zebra.MESSAGE_NEXTHOP | zebra.MESSAGE_DISTANCE | zebra.MESSAGE_METRIC,
	Prefix:       net.ParseIP("10.10.10.0"),
	PrefixLength: 24,
	Nexthops:     []net.IP{net.ParseIP("192.168.2.2")},
	Distance:     10,
	Metric:       20,
}

cli.SendCommand(zebra.IPV4_ROUTE_ADD, b)

cli.Close()
```
