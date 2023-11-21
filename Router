from scapy.all import ARP, Ether, srp
import socket

def get_mac_address(ip_address):
    try:
        mac_address = ARP(pdst=ip_address).hwsrc
        return mac_address
    except Exception as e:
        print(f"An error occurred: {e}")
        return None

def get_ip_address(domain_name):
    try:
        return socket.gethostbyname(domain_name)
    except socket.gaierror:
        print("Invalid domain name.")
        return None

def get_router_ip(domain_name):
    ip_address = get_ip_address(domain_name)
    mac_address = get_mac_address(ip_address)
    arp_request = ARP(pdst=ip_address)
    broadcast = Ether(dst="ff:ff:ff:ff:ff:ff")
    arp_request_broadcast = broadcast / arp_request
    answered, _ = srp(arp_request_broadcast, timeout=1, verbose=False)
    router_ip = answered[0][1].psrc
    return router_ip

domain_name = input("Please enter the domain name: ")
router_ip = get_router_ip(domain_name)

if router_ip:
    print("The IP address of the router is {}".format(router_ip))
