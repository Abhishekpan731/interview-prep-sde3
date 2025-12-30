# 150 OAuth2, JWT, SAML, and SSO Interview Questions (SDE3)

## 1. OAuth 2.0: Delegation & Authorization (40 Questions)
1. What were the primary security flaws in OAuth 1.0 that led to OAuth 2.0?
2. Explain the four roles in OAuth 2.0: **Resource Owner, Client, Authorization Server, and Resource Server**.
3. What is the difference between **Front-channel** and **Back-channel** communication?
4. Walk through the **Authorization Code Flow** in detail. Why is the `code` exchanged for a `token`?
5. What is **PKCE (Proof Key for Code Exchange)**? How does it protect against "Authorization Code Interception" attacks?
6. When do you use the **Client Credentials Flow**? Why is there no "Resource Owner" involved?
7. Explain the **Device Code Flow**. Where is it used?
8. Why is the **Implicit Flow** now officially deprecated? What were the security risks?
9. Explain the **Resource Owner Password Credentials (ROPC)** flow. Why is it an anti-pattern for modern apps?
10. What are **Scopes**? How do they differ from **Permissions** or **Roles**?
11. Explain **Consent**. What is the difference between "Incremental Consent" and "Admin Consent"?
12. What is an **Access Token**? Is it opaque or structured by the OAuth 2.0 spec?
13. What is a **Refresh Token**? How do you handle **Refresh Token Rotation**?
14. Explain **Token Introspection (RFC 7662)**. When is it preferred over local JWT validation?
15. What is **Token Revocation (RFC 7009)**?
16. How does the **State** parameter prevent CSRF attacks in OAuth 2.0?
17. Explain **Redirect URI** validation. What are the risks of "Wildcard Redirect URIs"?
18. What is the **Authorization Server Metadata (RFC 8414)** (Discovery Endpoint)?
19. Explain **Client Authentication** methods: `client_secret_basic`, `client_secret_post`, and `private_key_jwt`.
20. What is **Dynamic Client Registration (RFC 7591)**?
21. Explain **Mutual TLS (mTLS)** for OAuth 2.0.
22. What is **DPoP (Demonstrating Proof-of-Possession)**? How does it bind a token to a client's private key?
23. Explain **FAPI (Financial-grade API)** security profile.
24. What is **RAR (Rich Authorization Requests)** (RFC 9396)?
25. Explain **Step-up Authentication** in an OAuth 2.0 context.
26. How to handle **Multi-tenant OAuth 2.0** implementations?
27. What is the impact of **User Agent (Browser)** restrictions like ITP on OAuth flows?
28. Explain **OAuth 2.0 for Native Apps (RFC 8252)**. Why is a custom URI scheme used?
29. What is **PAR (Pushed Authorization Requests)** (RFC 9126)? How does it improve security?
30. Explain **JAR (JWT-Secured Authorization Request)**.
31. What is the **User Managed Access (UMA)** protocol built on OAuth 2.0?
32. How to implement **Rate Limiting** in an Authorization Server?
33. Explain the "Confused Deputy" problem in OAuth 2.0.
34. What is the difference between **Public Clients** and **Confidential Clients**?
35. How to handle **Token Exchange (RFC 8693)** in a microservices "on-behalf-of" scenario?
36. What is the **OAuth 2.0 Security Best Current Practice (BCP)**?
37. Explain **Resource Indicators** in OAuth 2.0.
38. How to implement **Audit Logging** for an OAuth 2.0 system?
39. What is the role of **Bearer Token**? What are the risks of it being stolen?
40. Discuss the future of OAuth (OAuth 2.1). What major changes are consolidated there?

## 2. OpenID Connect (OIDC): The Identity Layer (30 Questions)
41. What is **OpenID Connect (OIDC)**? How does it differ from OAuth 2.0?
42. Why can't OAuth 2.0 be used for Authentication alone?
43. What is an **ID Token**? What are its mandatory claims?
44. Explain the **Nonce** parameter in OIDC. How does it prevent replay attacks?
45. What is the **UserInfo Endpoint**? What data does it return?
46. Explain the **Standard Scopes** in OIDC: `openid`, `profile`, `email`, `address`, `phone`.
47. What is the **Discovery Document** (`.well-known/openid-configuration`)?
48. Explain **Claims Requesting** in OIDC.
49. What is **Hybrid Flow** in OIDC? (e.g., `response_type=code id_token`).
50. Explain **Self-Issued OpenID Provider (SIOP)**.
51. What is **Logout** in OIDC? Explain **Front-channel Logout** and **Back-channel Logout**.
52. What is **Session Management** in OIDC?
53. Explain **RP-Initiated Logout**.
54. What are the **Key Claims** in an ID Token: `iss`, `sub`, `aud`, `exp`, `iat`, `auth_time`?
55. What is the **acr (Authentication Context Class Reference)** claim?
56. Explain the **amr (Authentication Methods References)** claim.
57. What is the difference between the **subject (sub)** claim and the `user_id`?
58. Explain **Pairwise Subject Identifiers** and why they are used for privacy.
59. What is **Request Object (request parameter)** in OIDC?
60. Explain **ID Token Validation** steps on the client side.
61. What is **OIDC Federation**?
62. How does OIDC handle **MFA (Multi-Factor Authentication)**?
63. Explain **OpenID Connect Dynamic Client Registration**.
64. What is the purpose of the `at_hash` and `c_hash` claims?
65. How does OIDC support **Aggregated and Distributed Claims**?
66. What is the **Subject Type** (`public` vs. `pairwise`)?
67. Explain **OIDC for Verifiable Credentials**.
68. What is the impact of **Server-side vs. Client-side** OIDC implementation?
69. How to customize the **OIDC provider** (e.g., Keycloak/Okta) logic?
70. What is the **OpenID Certified** program?

## 3. JWT: JSON Web Tokens Internals (30 Questions)
71. What is a **JWT**? Explain its three parts (Header, Payload, Signature).
72. What is the difference between **JWS (JSON Web Signature)** and **JWE (JSON Web Encryption)**?
73. Explain the **Signing Algorithms**: `HS256` (Symmetric) vs. `RS256`/`ES256` (Asymmetric).
74. Why is `RS256` generally preferred over `HS256` in distributed systems?
75. What are the security risks of the `alg: none` header?
76. Explain **JWKS (JSON Web Key Set)** and the `jwks_uri`.
77. How does **Key Rotation** work without breaking existing tokens?
78. What is a **Kid (Key ID)** in a JWT header?
79. Explain **Self-contained Tokens** vs. **Reference Tokens**. What are the trade-offs?
80. How do you handle **JWT Revocation**? (Blacklisting, Whitelisting, Short TTLs).
81. What is the impact of **Clock Skew** on JWT validation?
82. How to handle **claims** that exceed the maximum header/URL size?
83. Explain **Nested JWTs**.
84. What is the **"sub" (subject)** claim? How should it be structured?
85. What is the **"aud" (audience)** claim? Why is it critical to validate?
86. Explain **Typed JWTs** (`typ: JWT`).
87. How to prevent **XSS and CSRF** when storing JWTs in a browser (Local Storage vs. Cookies)?
88. What is a **Replay Attack** on a JWT?
89. How to encrypt a JWT using **JWE**? When is it necessary?
90. Explain the **Crit (Critical)** header claim.
91. What is the **exp (Expiration Time)** vs. **nbf (Not Before)** vs. **iat (Issued At)**?
92. How does **JWT performance** compare to opaque tokens?
93. What is the maximum size recommended for a JWT?
94. Explain **Token Binding** for JWTs.
95. How to handle **User Metadata** inside a JWT?
96. What is **compact serialization** vs. **JSON serialization**?
97. Explain **Jose (Javascript Object Signing and Encryption)** family of specs.
98. What is the **Thumbprint** of a JWT key?
99. How to debug a JWT? (jwt.io vs. local tools).
100. Discuss **JWT Security vulnerabilities** beyond the "alg: none" (e.g., Key Confusion).

## 4. SAML 2.0: Enterprise Identity (25 Questions)
101. What is **SAML (Security Assertion Markup Language)**?
102. Explain the roles: **Identity Provider (IdP)** and **Service Provider (SP)**.
103. Walk through the **SAML Web Browser SSO Profile (SP-Initiated)**.
104. What is **IdP-Initiated SSO**? What are its security risks (CSRF)?
105. What is a **SAML Assertion**? Describe its internal XML structure.
106. Explain **SAML Bindings**: `HTTP Redirect`, `HTTP POST`, and `SOAP`.
107. What is **SAML Metadata**? Why are XML signatures crucial here?
108. Explain the **Assertion Consumer Service (ACS) URL**.
109. What is the **RelayState** parameter?
110. Explain **SAML Subject NameID**. What are the various formats (`emailAddress`, `persistent`, `transient`)?
111. What is the **AttributeStatement** in a SAML assertion?
112. Explain **Digital Signatures in SAML**. What do you sign (Assertion, Response, or both)?
113. What is **Enveloped Signature**?
114. How does SAML handle **Encryption**?
115. Explain **SAML Single Logout (SLO)**.
116. What is **Artifact Resolution Profile**? Why is it used?
117. Compare **SAML vs. OAuth 2.0**.
118. Compare **SAML vs. OIDC**.
119. What is a **Circle of Trust** in SAML?
120. Explain **InCommon** and other SAML federations.
121. How to handle **Certificate Expiry** in a SAML federation?
122. What is **SAML Proxying**?
123. Explain **AuthnRequest** attributes like `ForceAuthn` and `IsPassive`.
124. What is the **Entity ID**?
125. Explain the **SAML Security vulnerabilities** (e.g., XML Signature Wrapping - XSW).

## 5. SSO & Federation Masterclass (25 Questions)
126. What is **Single Sign-On (SSO)**? How does it differ from **Same Sign-On**?
127. Explain **Web SSO** vs. **Enterprise SSO**.
128. What is **Identity Federation**?
129. How does **Social Login** (Log in with Google/Facebook) work under the hood?
130. Explain **BFF (Backend For Frontend)** security pattern for SSO.
131. What is a **Session Cookie** vs. an **Identity Token**?
132. How do you implement SSO across **Multiple Domains**? (e.g., app1.com and app2.com).
133. Explain **Desktop SSO / Kerberos / WIA (Windows Integrated Authentication)**.
134. What is **Active Directory Federation Services (ADFS)**?
135. How to handle **SSO Session Expiry** vs. **Application Session Expiry**?
136. What is **Single Logout (SLO)**? Why is it technically challenging to achieve perfectly?
137. Explain **Step-up Authentication** in an SSO environment.
138. What is **Adaptive SSO**?
139. How to implement **MFA (Multi-Factor Authentication)** as part of the SSO flow?
140. Explain **IdP Discovery (Home Realm Discovery)**.
141. What is **Just-In-Time (JIT) Provisioning** during an SSO login?
142. Explain **SCIM (System for Cross-domain Identity Management)** and its relationship with SSO.
143. How to handle **SSO for Legacy Applications**?
144. What is **Passwordless Authentication** (WebAuthn/FIDO2) integration with SSO?
145. Explain **Conditional Access Policies**.
146. How to monitor and audit **SSO Logins**?
147. What is **IdP Chaining**?
148. How to perform a **Security Audit** of an SSO implementation?
149. Discuss **Tenant Isolation** in a multi-tenant SSO setup.
150. Future of Identity: **Decentralized Identity (DID)** and **Self-Sovereign Identity (SSI)**.

---
*Generated by Antigravity for SDE3 Interview Prep.*
