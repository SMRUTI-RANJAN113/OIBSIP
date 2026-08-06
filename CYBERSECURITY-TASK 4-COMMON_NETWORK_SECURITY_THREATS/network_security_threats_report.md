# Common Network Security Threats: A Research Report

## Introduction

Modern organizations run almost every critical function — banking, healthcare, logistics, communication — over interconnected networks, which makes those networks a prime target for attackers. A single successful network-level attack can take a global service offline, redirect millions of dollars in transactions, or expose sensitive data without a single endpoint ever being "hacked" in the traditional sense. Unlike malware that must be installed on a victim's machine, many network attacks succeed simply by abusing the trust built into core internet protocols such as TCP/IP, DNS, and BGP — protocols designed in an era when the internet was small and mostly trusted. As networks have scaled and threat actors have professionalized, understanding how these attacks work, what they cost when they succeed, and how to defend against them has become a baseline requirement for any network administrator or security professional.

## 1. DoS/DDoS Attacks

**How it works:** A Denial-of-Service (DoS) attack floods a target system, server, or network with traffic or requests until it can no longer respond to legitimate users. A Distributed Denial-of-Service (DDoS) attack does the same thing but uses many source machines at once — often a botnet of compromised devices, or in the case of amplification attacks, third-party servers that are tricked into flooding the victim on the attacker's behalf. In a reflection/amplification attack, the attacker sends a small request to a misconfigured server (such as a memcached, DNS, or NTP server) while spoofing the victim's IP address as the source; the server then sends a much larger response to the victim, multiplying the attacker's bandwidth many times over.

**Real-world example:** On February 28, 2018, GitHub was hit by what was, at the time, the largest DDoS attack ever recorded, peaking at 1.35 terabits per second and 126.9 million packets per second. Attackers abused publicly exposed memcached servers, spoofing GitHub's IP address in small requests that caused each server to send back a response tens of thousands of times larger. The attack originated from over a thousand different autonomous systems and briefly knocked GitHub offline, though the site recovered within about ten minutes with help from DDoS mitigation provider Akamai.

**Impact:** GitHub's outage lasted only minutes because the company had pre-arranged DDoS scrubbing capacity, but the incident demonstrated that misconfigured, internet-exposed servers running protocols like memcached can be weaponized to generate record-breaking traffic volumes without attackers needing a botnet at all — a technique that has since been reused against many other targets.

**Mitigation strategies:**
1. Deploy a DDoS mitigation/scrubbing service or CDN (e.g., Cloudflare, Akamai, AWS Shield) that can absorb and filter volumetric traffic before it reaches origin servers.
2. Configure rate limiting and traffic filtering at the network edge, and disable or firewall off unnecessary UDP-based services (like memcached and NTP) so they cannot be abused as amplification reflectors.
3. Maintain an incident response and traffic-monitoring plan (anomaly detection on ingress/egress ratios) so on-call teams can detect and reroute traffic within minutes, as GitHub's monitoring system did.

## 2. Man-in-the-Middle (MITM) Attacks

**How it works:** In a MITM attack, an adversary secretly positions itself between two communicating parties — for example, a user's browser and a website — and intercepts, reads, or alters the traffic without either party realizing it. This is often achieved by compromising a certificate authority trust chain, exploiting insecure Wi-Fi, ARP spoofing on a local network, or installing malicious software that intercepts encrypted (HTTPS/TLS) traffic by inserting a fraudulent root certificate into the victim's device.

**Real-world example:** In late 2014 and early 2015, Lenovo shipped consumer laptops preloaded with adware called Superfish. Superfish installed its own self-signed root certificate into Windows and then re-signed every HTTPS website a user visited with that same certificate, allowing it to decrypt and inspect encrypted traffic to inject ads. Because Superfish used the same private key across all affected laptops, once security researchers extracted that key, any attacker on the same network as a Superfish-equipped laptop — for instance, on public Wi-Fi — could forge valid-looking certificates for any website, including banks, and intercept that user's supposedly secure traffic without triggering a browser warning. CISA issued a public alert warning that the flaw left users vulnerable to HTTPS spoofing.

**Impact:** The Superfish incident affected an estimated hundreds of thousands of Lenovo consumer laptops sold in a several-month window, undermined the certificate-based trust model that HTTPS depends on, and forced Lenovo to issue a public apology, release a removal tool, and face regulatory scrutiny and lawsuits over the practice.

**Mitigation strategies:**
1. Enforce HTTPS everywhere with HSTS (HTTP Strict Transport Security) so browsers refuse to downgrade to unencrypted or self-signed connections.
2. Use certificate pinning for critical applications and regularly audit the certificate authorities trusted by managed devices, removing any unauthorized or suspicious root certificates.
3. Avoid conducting sensitive transactions over untrusted public Wi-Fi, or require a VPN with strong encryption when doing so, so a local attacker cannot easily insert themselves into the traffic path.

## 3. IP Spoofing

**How it works:** IP spoofing is the practice of crafting network packets with a forged source IP address, making traffic appear to originate from a trusted or different host than it actually does. Because early TCP/IP implementations did not verify that a packet's claimed source address was genuine, spoofing could be combined with predictable TCP sequence numbers to hijack a trusted connection entirely — impersonating a machine that the target already trusted, without needing a password.

**Real-world example:** On Christmas Day 1994, Kevin Mitnick attacked the home computer of security researcher Tsutomu Shimomura using IP spoofing combined with TCP sequence number prediction. Mitnick first used a SYN-flood to disable a trusted server so it could not respond to legitimate traffic, then studied the target's pattern of TCP initial sequence numbers to predict the numbers it would generate, and finally sent packets spoofed with the trusted server's IP address. Because Shimomura's target machine believed it was communicating with the already-trusted server, it granted access without requiring a password. The intrusion was captured in detail because Shimomura had been logging his network traffic, and the resulting investigation eventually helped lead the FBI to Mitnick's arrest in February 1995.

**Impact:** Beyond compromising one researcher's machine, the case became one of the most widely studied network intrusions in security history, exposing how the industry's reliance on IP-address-based trust (without random, unpredictable TCP sequence numbers) could be systematically defeated, and it pushed vendors to redesign TCP/IP stacks with randomized sequence number generation.

**Mitigation strategies:**
1. Deploy ingress/egress filtering (e.g., BCP 38) at network boundaries so routers drop packets whose source address doesn't plausibly belong to the network they arrived from.
2. Use randomized, cryptographically unpredictable TCP initial sequence numbers (a standard feature in modern operating systems) so sequence prediction attacks are no longer feasible.
3. Avoid IP-address-based authentication for trust relationships between machines; require cryptographic authentication (e.g., mutual TLS, SSH keys) instead of relying on "this IP is trusted."

## 4. DNS Poisoning / Spoofing (Bonus Threat)

**How it works:** DNS poisoning (or spoofing) corrupts the DNS resolution process so that a domain name resolves to an attacker-controlled IP address instead of the legitimate one. This can happen by injecting forged DNS responses into an unauthenticated resolver's cache, or, at a larger scale, by hijacking the routing itself — for example through a Border Gateway Protocol (BGP) hijack — so that traffic intended for a legitimate DNS provider is rerouted to servers the attacker controls, letting them return whatever DNS answers they want.

**Real-world example:** On April 24, 2018, attackers executed a BGP hijack against Amazon's Route 53 DNS infrastructure, redirecting DNS traffic for the cryptocurrency wallet service MyEtherWallet.com to servers the attackers controlled in Russia. For about two hours, users attempting to reach the real MyEtherWallet site were instead sent to a convincing phishing replica; those who ignored a browser warning about the site's untrusted TLS certificate had their private keys stolen. The attackers made off with roughly $150,000–$160,000 worth of Ether before MyEtherWallet regained control of its DNS records.

**Impact:** The incident showed that even a well-secured application (MyEtherWallet's own servers were never breached) can be compromised if the underlying routing and DNS infrastructure it depends on is hijacked, and it renewed industry calls for wider adoption of DNSSEC and route-origin validation, since ordinary users have almost no way to detect a BGP-level hijack themselves.

**Mitigation strategies:**
1. Implement DNSSEC to cryptographically sign DNS records, so resolvers can detect and reject forged responses.
2. Adopt Resource Public Key Infrastructure (RPKI) and route-origin validation to make BGP route hijacks harder for networks to get away with.
3. Enforce HSTS and valid-certificate checks on the client side so that even if DNS is spoofed, browsers refuse to silently proceed to a site presenting an untrusted or mismatched certificate.

## Comparison Table

| Threat | Attack Vector | Who Is at Risk | Difficulty to Execute | Ease of Mitigation |
|---|---|---|---|---|
| DoS/DDoS | Volumetric traffic flooding, often via amplification/reflection using spoofed source IPs | Any internet-facing service; especially high-profile or under-provisioned sites | Low–Medium (amplification tools and DDoS-for-hire services are widely available) | Medium (requires scrubbing capacity/CDN, ongoing cost) |
| Man-in-the-Middle | Interception of traffic via rogue certificates, insecure Wi-Fi, or ARP spoofing | Users on untrusted networks; any HTTPS traffic if trust chain is compromised | Medium (easier on local/public networks, harder against fully hardened TLS) | Medium–High (HSTS, cert pinning largely close the gap) |
| IP Spoofing | Forging the source IP address of packets, often combined with sequence prediction | Systems using IP-based trust/authentication; legacy or misconfigured networks | Medium (modern OS defenses have raised the bar since the 1990s) | High (ingress filtering + random sequence numbers are standard today) |
| DNS Poisoning/Spoofing | Corrupting DNS responses or hijacking routing (e.g., BGP) to redirect domain resolution | Any domain relying on unsigned DNS; end users of that domain | Medium–High (BGP hijacks require route-level access or ISP cooperation) | Medium (DNSSEC/RPKI adoption is still incomplete across the internet) |

## Conclusion

Three key takeaways for a network administrator emerge from these cases. First, many of the most damaging network attacks don't exploit a single buggy application — they exploit foundational trust assumptions baked into IP, TCP, and DNS, which means defenses have to be built at the protocol and infrastructure level (ingress filtering, DNSSEC, RPKI, HSTS) rather than relying solely on application-layer security. Second, exposure often comes from misconfiguration and neglected internet-facing services — an open memcached server, an unpatched adware certificate, an unsigned DNS zone — so regular audits of what is reachable from the internet, and why, are one of the highest-leverage things a team can do. Third, detection speed matters as much as prevention: GitHub survived a record-breaking DDoS with only minutes of downtime because monitoring and mitigation were already in place, while MyEtherWallet users lost funds during a two-hour window before the hijack was resolved — underscoring that a tested incident response plan is not optional.

## References

1. National Institute of Standards and Technology (NIST) — nist.gov
2. Cybersecurity and Infrastructure Security Agency (CISA), "Lenovo Superfish Adware Vulnerable to HTTPS Spoofing," Alert TA15-051A — https://www.cisa.gov/news-events/alerts/2015/02/20/lenovo-superfish-adware-vulnerable-https-spoofing
3. GitHub Engineering Blog, "February 28th DDoS Incident Report" — https://github.blog/news-insights/company-news/ddos-incident-report/
4. MITRE ATT&CK Framework — https://attack.mitre.org
5. Help Net Security, "MyEtherWallet users robbed after successful DNS hijacking attack" — https://www.helpnetsecurity.com/2018/04/25/myetherwallet-dns-hijacking/
6. SEED Labs (Wenliang Du), "The Mitnick Attack Lab" — https://seedsecuritylabs.org/Labs_16.04/PDF/Mitnick_Attack.pdf
