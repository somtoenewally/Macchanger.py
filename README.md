# Macchanger.py
Simple mac chnager with python
#!/usr/bin/env python

import subprocess
import optparse
import re


def get_arguments():
    parser = optparse.OptionParser()
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change its MAC address")  
    parser.add_option("-m", "--mac", dest="new_mac", help="New MAC address")
    (options, arguments) = parser.parse_args()
    if not options.interface:
        #code to handle error
        parser.error("[+] Please specify an interface, use --help or more info.")
    elif not options.new_mac:
        #code to handle error
        parser.error("[+] Please specify an interface, use --help or more info.")
    return options
        


def change_mac(interface, new_mac):
    print("[+] changing Mac address for " + "to " + new_mac)
    ip_result = subprocess.check_output(["ip", "address","show", options.interface])
    subprocess.call(["ip", "link", "set", interface, "down"])
    subprocess.call(["ip", "link", "set", interface, "address",new_mac])
    subprocess.call(["ip", "link", "set", interface,"up"])

def get_current_mac(interface):
    ip_result = subprocess.check_output(["ip", "address","show", interface])
    mac_address_search_result = re.search(b"\w\w:\w\w:\w\w:\w\w:\w\w:\w\d" , ip_result)
    if mac_address_search_result:
        print(mac_address_search_result.group(0))
    else:
        print("[-] could not read MAC address")
