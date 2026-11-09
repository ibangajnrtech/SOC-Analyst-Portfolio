# Basic Packet Capture

You can run tcpdump without providing any arguments; however, this is only useful to test that you have it installed!

## Specify the Network Interface

The first thing to decide is which network interface to listen to using `-i INTERFACE`.
You can choose to listen on all available interfaces using `-i any`; alternatively, you can specify an interface you want to listen on, such as `-i eth0`.
A command such as `ip address show` (or merely `ip a s`) would list the available network interfaces.

## Save the Captured Packets

You can check the packets again by saving to a file using `-w FILE`.
The file extension is most commonly set to `.pcap`.
The saved packets can be inspected later using another program, such as Wireshark.

## Read Captured Packets from a File

You can use Tcpdump to read packets from a file by using `-r FILE`.
This is very useful for learning about protocol behaviour.
You can capture network traffic over a suitable time frame to inspect a specific protocol, then read the captured file while applying filters to display the packets you are interested in.

## Limit the Number of Captured Packets

You can specify the number of packets to capture by specifying the count using `-c COUNT`.
Without specifying a count, the packet capture will continue till you interrupt it, for example, by pressing CTRL-C.

## Don't Resolve IP Addresses and Port Numbers

To avoid making such DNS lookups, you can use the `-n` argument.
If you don’t want port numbers to be resolved, such as 80 being resolved to http, you can use the `-nn` to stop both DNS and port number lookups. 

## Produce (More) Verbose Output

If you want to print more details about the packets, you can use `-v` to produce a slightly more verbose output.

## Summary of tcpdump command line options

- `tcpdump -i INTERFACE`: Captures packets on a specific network interface
- `tcpdump -w FILE`: Writes captured packets to a file
- `tcpdump -r FILE`: Reads captured packets from a file
- `tcpdump -c COUNT`: Captures a specific number of packets
- `tcpdump -n`: Don’t resolve IP addresses
- `tcpdump -nn`: Don’t resolve IP addresses and don’t resolve protocol numbers
- `tcpdump -v`: Verbose display; verbosity can be increased with `-vv` and `-vvv`

# Filtering expressions

You can run tcpdump without providing any filtering expressions, this won’t be useful.

## Filtering by Host

- Use `tcpdump host IP` or `tcpdump host HOSTNAME` to easily limit the captured packets to a specific host or IP address.
- Use `tcpdump src host IP` or `tcpdump src host HOSTNAME` to limit the packets to those from a particular source IP address or hostname
- Use `tcpdump dst host IP` or `tcpdump dst host HOSTNAME` to limit packets to those sent to a specific destination

## Filtering by Port

- Use `tcpdump port PORT_NUMBER` to capture all traffic for a specific port (53 for DNS, 443 for HTTPS)
- Use `tcpdump src port PORT_NUMBER` and `tcpdump dst port PORT_NUMBER` to limit the packets to those from a particular source port number or to a particular destination port number 

## Filtering by Protocol

- Use `tcpdump PROTOCOL` to limit your packet capture to a specific protocol

## Logical Operators

- `and`: Captures packets where both conditions are true. For example, `tcpdump host 1.1.1.1 and tcp` captures `tcp` traffic with `host 1.1.1.1`.
- `or`: Captures packets when either one of the conditions is true. For instance, `tcpdump udp or icmp` captures `UDP` or `ICMP` traffic.
- `not`: Captures packets when the condition is not true. For example, `tcpdump not tcp` captures all packets except `TCP` segments; we expect to find UDP, ICMP, and ARP packets among the results.

## Summary of Command Line options

- `tcpdump host IP` or `tcpdump host HOSTNAME` filters packets by IP address or Hostname
- `tcpdump src host IP` or `tcpdump src host HOSTNAME` filters packets by a specific source host
- `tcpdump dst host IP` or `tcpdump dst host HOSTNAME` filters packets by a specific destination host
- `tcpdump port PORT_NUMBER` filters packets by a specific destination host
- `tcpdump src port PORT_NUMBER` filters packets by the specified source port number
- `tcpdump dst port PORT_NUMBER` filters packets by the specified destination port number
- `tcpdump PROTOCOL` filters packets by protocol; examples include `ip`, `ip6` and `icmp`

# Advanced Filtering

- Use `tcpdump greater LENGTH` to filter packets that have a length greater than or equal to the specified bytes
- Use `tcpdump less LENGTH` to filter packets that have a length lesser than or equal to the specified bytes
- Use `tcpdump "tcp[tcpflags] == tcp-syn"` to  capture TCP packets with only the SYN (Synchronize) flag set, while all the other flags are unset
- Use `tcpdump "tcp[tcpflags] & tcp-syn != 0"` to capture TCP packets with at least the SYN (Synchronize) flag set.
- Use `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"` to capture TCP packets with at least the SYN (Synchronize) or ACK (Acknowledge) flags set

You can use tcp[tcpflags] to refer to the TCP flags field. The following TCP flags are available to compare with:
- `tcp-syn` TCP SYN (Synchronize)
- `tcp-ack` TCP ACK (Acknowledge)
- `tcp-fin` TCP FIN (Finish)
- `tcp-rst` TCP RST (Reset)
- `tcp-push` TCP Push

# Displaying Packets

- `-q`: Quick output; print brief packet information
- `-e`: Print the link-level header
- `-A`: Show packet data in ASCII
- `-xx`: Show packet data in hexadecimal format, referred to as hex
- `-X`: Show packet headers and data in hex and ASCII

