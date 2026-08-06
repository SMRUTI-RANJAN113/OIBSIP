# Social Engineering Attacks: Phishing, Pretexting, and Baiting

## Introduction

Social engineering is the practice of manipulating people — rather than machines — into breaking normal security procedures, handing over confidential information, or granting unauthorized access. It exploits fundamentally human traits: trust, urgency, curiosity, helpfulness, and fear of authority. What makes social engineering one of the most effective attack vectors in cybersecurity is that it routinely sidesteps the strongest technical controls an organization can buy. Firewalls, encryption, and multi-factor authentication don't help if an employee is simply persuaded to wire money to a fraudulent account or plug in an infected USB drive. Industry incident reports consistently identify the "human element" — phishing, misuse, and social engineering combined — as involved in the majority of confirmed data breaches, and business email compromise (a form of phishing/pretexting) alone has cost victims tens of billions of dollars globally over the past decade, according to FBI Internet Crime Complaint Center reporting. This report examines the three attack types named in the brief — phishing, pretexting, and baiting — plus quid pro quo as a bonus category, each with a documented real-world case and concrete prevention measures.

## Phishing

**Definition:** Phishing is the use of fraudulent communications — most often email, but also text messages, voice calls, or social media — that appear to come from a trusted source, in order to trick a target into revealing sensitive information, clicking a malicious link, or authorizing a fraudulent transaction.

**Types:**
- **Spear phishing** — a targeted attack aimed at a specific individual or organization, using personal or organizational details to appear more credible than a generic mass email.
- **Whaling** — spear phishing aimed specifically at senior executives or other high-value targets ("big fish"), often impersonating a CEO or board member to authorize wire transfers or release sensitive data.
- **Vishing** — "voice phishing," carried out over phone calls or voicemail, often impersonating a bank, IT helpdesk, or government agency.
- **Smishing** — phishing carried out via SMS text message, frequently containing malicious links or spoofed delivery/bank notifications.

**How it works:** The attacker crafts a message that looks like it comes from a legitimate, trusted party — a colleague, vendor, bank, or well-known brand — and creates a sense of urgency or authority that discourages the victim from scrutinizing the request. The message typically directs the victim to a spoofed login page that harvests credentials, an attachment that installs malware, or a request to transfer funds or data directly.

**Real-world case study:** Between 2013 and 2015, Lithuanian national Evaldas Rimasauskas ran a business email compromise scheme that tricked employees at Google and Facebook into wiring more than $100 million combined to bank accounts he controlled. Rimasauskas registered a company in Latvia under a name nearly identical to Quanta Computer, a real Taiwan-based hardware supplier that both tech giants did business with, and sent phishing emails with forged invoices, contracts, and letters bearing fake corporate stamps that appeared to be signed by real Quanta executives, directing payment to his own accounts in Latvia and Cyprus instead of Quanta's genuine accounts. The scheme defrauded Facebook of $99 million and Google of $23 million before it was uncovered; Facebook recovered most of its funds, and Rimasauskas was eventually extradited to the U.S., pleaded guilty to wire fraud, and was sentenced to five years in prison plus tens of millions of dollars in forfeiture and restitution.

**Prevention recommendations:**
1. Require out-of-band verification (a phone call to a known, previously verified number) for any request to change payment details or wire funds, regardless of how official the email looks.
2. Deploy email authentication protocols — SPF, DKIM, and DMARC — to reduce the ability of attackers to spoof legitimate domains.
3. Run regular, realistic phishing simulation exercises so employees learn to recognize spoofed domains, urgency tactics, and unusual payment requests.
4. Enforce multi-factor authentication (MFA) on email and financial systems so that a stolen password alone is not enough to compromise an account.

## Pretexting

**Definition:** Pretexting is a social engineering technique in which an attacker fabricates a false scenario ("pretext") — often impersonating a co-worker, authority figure, or trusted third party — to convince a target to disclose information or take an action they wouldn't otherwise take.

**How an attacker builds a false scenario:** The attacker first researches the target organization to learn names, job titles, internal jargon, and processes that make the pretext believable. They then construct a plausible identity — such as an IT technician, auditor, journalist, or fellow employee — and a believable reason for the request, such as "verifying an account for a security audit" or "confirming records on behalf of a board member." Because the interaction typically happens over the phone or in writing rather than through a system login, technical defenses have little to catch, and the success of the attack depends entirely on whether the target believes the fabricated identity and story.

**Real-world case study:** In 2006, Hewlett-Packard's leadership, under then-chairwoman Patricia Dunn, hired private investigators to uncover the source of boardroom leaks to journalists. Those investigators used pretexting, impersonating HP board members, employees, and journalists — including providing operators at phone companies with victims' Social Security numbers obtained elsewhere — to trick telecom customer service representatives into releasing private phone records. The pretexting effort ultimately targeted the phone records of nine journalists and numerous HP board members, employees, and even family members. When the practice became public via a Wall Street Journal report and congressional hearings in 2006, it forced Dunn's resignation as chair, triggered criminal charges against HP executives and the investigators involved, and led directly to the U.S. Congress passing the Telephone Records and Privacy Protection Act, making pretexting for phone records a federal crime.

**Prevention recommendations:**
1. Train frontline staff (helpdesk, customer service, reception) to independently verify a caller's identity through a separate, pre-established channel before releasing any information, regardless of how much detail the caller already seems to know.
2. Establish strict, documented procedures for identity verification (e.g., account PINs, callback to a number on file) that employees cannot be talked out of following, even under pressure or apparent authority.
3. Limit how much organizational and personal information is publicly available (job titles, org charts, internal terminology) that an attacker could use to construct a convincing pretext.

## Baiting

**Definition:** Baiting lures a victim into a security compromise by offering something enticing — a physical item or a digital download — that secretly delivers malware once accessed.

**Physical baiting:** The classic example is leaving infected USB drives in a location where curious people will find them — a parking lot, lobby, or break room — often labeled with tempting text like "Confidential" or "Payroll 2026" to increase the odds someone plugs it in.

**Digital baiting:** The equivalent online is offering free downloads, pirated software, "too good to be true" giveaways, or fake browser/software updates that actually install malware when the victim runs them.

**Real-world case study:** In 2008, a foreign intelligence service reportedly left malware-infected USB flash drives in the parking lot of a U.S. military facility in the Middle East. Once an employee found one and plugged it into a laptop connected to U.S. Central Command's network, a malicious worm spread undetected across both classified and unclassified military networks, giving attackers a foothold that took the Department of Defense roughly 14 months to fully clean out in an operation later known as "Buckshot Yankee." The incident is widely credited with prompting the U.S. military's temporary ban on USB drives and helping spur the creation of U.S. Cyber Command. Separately, controlled research studies — including one by a Google researcher and the University of Illinois that dropped nearly 300 USB drives across a university campus — have found that roughly half of people who find an unfamiliar USB drive will plug it into a computer, often within minutes, illustrating just how reliable baiting is as an attack technique.

**Prevention recommendations:**
1. Disable or restrict USB ports on corporate devices through endpoint security policy, allowing only pre-approved, encrypted removable media.
2. Train employees to never plug in an unknown USB device and to instead hand it to the IT/security team for safe inspection.
3. Deploy endpoint detection and response (EDR) tools that can flag or block unauthorized removable media and unusual process behavior the moment a device is connected.

## Quid Pro Quo (Bonus)

**Explanation:** Quid pro quo attacks offer a service or benefit in exchange for information or access — for example, an attacker cold-calling employees pretending to be IT support offering to "fix" a problem, in exchange for the employee's login credentials or a moment of remote access to their machine. Unlike baiting, which dangles an item, quid pro quo dangles a service or favor, exploiting the target's willingness to reciprocate help.

**Prevention:** Establish a clear, well-publicized policy that legitimate IT support never asks for a password over the phone, require employees to verify any unsolicited "support" offer through the official IT helpdesk channel before granting access, and log and review all remote-access sessions granted to internal support staff.

## Comparison Table

| Attack Type | Primary Target | Psychological Lever Exploited | Best Countermeasure |
|---|---|---|---|
| Phishing | Employees with email/financial access | Urgency, authority, trust in familiar brands or contacts | Out-of-band verification + email authentication (SPF/DKIM/DMARC) + MFA |
| Pretexting | Customer service reps, helpdesk, gatekeepers of records | Trust in authority, deference to a fabricated identity | Independent identity verification via a separate, pre-established channel |
| Baiting | Curious or helpful individuals (any employee) | Curiosity, desire to help find an owner or grab a freebie | Disable/restrict removable media; employee awareness training |
| Quid Pro Quo | Employees seeking help or convenience | Reciprocity, desire for a quick fix | Verify unsolicited "support" offers through official channels only |

## Organisational Recommendations: 5-Point Employee Security Awareness Training Checklist

1. **Run realistic, recurring phishing and vishing simulations** so employees build pattern recognition for suspicious requests rather than relying on one-time training.
2. **Teach the "verify, don't trust" principle** — any request involving money, credentials, or sensitive data must be verified through a separate, known channel, no matter how urgent or authoritative it appears.
3. **Make reporting easy and blame-free** — provide a one-click "report phishing" button and encourage employees to report suspicious calls, emails, or found devices without fear of embarrassment or punishment.
4. **Cover physical and non-email vectors explicitly** — training should include pretexting phone calls, baited USB drives, and quid pro quo offers, not just email-based phishing.
5. **Reinforce policy with technical backstops** — pair training with MFA, restricted USB ports, email authentication, and strict identity-verification procedures so a single lapse in judgment doesn't become a full compromise.

## References

1. National Institute of Standards and Technology (NIST) — nist.gov
2. Cybersecurity and Infrastructure Security Agency (CISA) — cisa.gov
3. SANS Institute Reading Room — sans.org/reading-room
4. MITRE ATT&CK Framework, Social Engineering techniques — https://attack.mitre.org
5. NPR, "Man Pleads Guilty To Phishing Scheme That Fleeced Facebook, Google Of $100 Million" — https://www.npr.org/2019/03/25/706715377/man-pleads-guilty-to-phishing-scheme-that-fleeced-facebook-google-of-100-million
6. CIO.com, "HP spying scandal: a timeline" — https://www.cio.com/article/260587/hp-spying-scandal-a-timeline.html
7. ManageEngine DataSecurity Plus, "USB Drop Attack" — https://www.manageengine.com/data-security/security-threats/usb-drop-attack.html
