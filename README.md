# pfSense DoH/DoT Blocklist

A consolidated, deduplicated, and auto-updated blocklist of DNS-over-HTTPS (DoH) and DNS-over-TLS (DoT) servers, designed specifically for **pfSense** firewalls.

This project merges multiple trusted sources into two clean files (IPs and Domains) ready for use in pfSense **URL Table (IPs)** aliases.

## 📋 Sources

This list aggregates data from the following upstream providers:

### IP Addresses
- **HaGeZi**: `ips/doh.txt` (DoH Endpoint IPs)
- **HaGeZi**: `ips/nameserver.txt` (Public Nameserver IPs)
- **crypt0rr**: `ipv4.list` & `ipv6.list` (Public DoH Server IPs)

### Domains
- **HaGeZi**: `domains/doh.txt`
- **HaGeZi**: `wildcard/doh-vpn-proxy-bypass-onlydomains.txt`
- **HaGeZi**: `wildcard/doh-onlydomains.txt`
- **crypt0rr**: `dns.list`

## 🚀 Usage in pfSense

### 1. Create Aliases
Navigate to **Firewall > Aliases > Add**.

#### For IP Blocking
- **Name**: `DOH_Block_IPs`
- **Type**: `URL Table (IPs)`
- **URL**: `https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/doh_ips.txt`
- **Update Frequency**: `1` Day

#### For Domain Blocking (Optional)
- **Name**: `DOH_Block_Domains`
- **Type**: `URL Table (IPs)` (pfSense will resolve these to IPs)
- **URL**: `https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/doh_domains.txt`
- **Update Frequency**: `1` Day

> **Note**: Replace `YOUR_USERNAME` and `YOUR_REPO` with your actual GitHub details.

### 2. Create Firewall Rules
Navigate to **Firewall > Rules > LAN** and create the following **Block** rules (place them at the top of the list):

| Action | Protocol | Destination | Destination Port | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Block** | TCP/UDP | `DOH_Block_IPs` | `53` | Block Standard DNS Bypass (IPs) |
| **Block** | TCP | `DOH_Block_IPs` | `853` | Block DoT (IPs) |
| **Block** | TCP | `DOH_Block_IPs` | `443` | Block DoH (IPs) |

*(Repeat these rules for `DOH_Block_Domains` if you created the domain alias.)*

## 🔄 Auto-Updates
This repository uses a **GitHub Action** to automatically fetch, merge, and deduplicate the source lists every 24 hours. No manual intervention is required.

## ⚠️ Disclaimer
This list is provided "as is" for educational and network administration purposes. While efforts are made to ensure accuracy, false positives can occur. The maintainers are not responsible for any network disruptions or damages resulting from the use of these lists. Always test in a non-production environment first.

## 📄 License
This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

## 🙏 Credits
- **HaGeZi**: For comprehensive DNS bypass and threat intelligence lists.
- **crypt0rr**: For maintaining a directory of public DoH servers.   
