#### Normal Firewall (Layer 3/4/5)
![[AWS Layer Firewall (L7)-1.png]]

Layer 3/4 firewall treat connection as separate request and response, different and unrelated.

Layer 5 firewall treat connection as the same using session. Reduce admin overhead and allows for more "contextual" security.

But both of Layer 3/4 and Layer 5 firewall cannot see the above layer data (e.g Level 6 and 7 HTTP/HTTPS). To them the cat image is the same as the dog image, and same as the malware -> security risk.
#### Layer 7 Architect
![[AWS Layer Firewall (L7)-2.png]]
- *Layer 7* firewall **understand HTTP/HTTPS**.
- It can *inspected* and *blocked*, *replaced* or *tagger* (adult, spam, off topic) data.
- Able to *identify*, *block*, *adjust* specific applications e.g Facbook, Instagram.
- *Keep all the L3, L4, L5 features* - But can understand L7 (DNS, Rate, Contents, Headers, ...).
- *Encryption* (HTTPS) **terminated** (decrypted) *on the L7 firewall*, and *firewall* **created a new connection** between Firewall and Backend.
[[Web Application Firewall (WAF)]]



