<div class="breaker"></div>
<a name="fal"></a>

## 7. Federation Assurance Level (FAL)

*This section is normative.*

This section defines allowable Federation Assurance Levels, or FAL. The FAL describes requirements for how assertions and constructed and secured for a given transaction. These levels can be requested by an RP or required by the configuration of both the RP and the IdP for a given transaction. 

All assertions SHALL comply with the detailed requirements in [Section 5](#sec5). While many other federation implementation options are possible, this list is intended to provide clear implementation recommendations representing increasingly secure deployment choices. Combinations of aspects not found in the FAL table are possible but outside the scope of this document.

This table presents different requirements for each FAL. Each successive level subsumes and fulfills all requirements of lower levels. Federations presented through a proxy SHALL be represented by the lowest level used during the proxied transaction.

<a name="63cSec7-Table1"></a>

<div class="text-center" markdown="1">


**Table 7-1. Federation Assertion Levels**

</div>

|FAL|Requirement|
|:--:|----|----|
|1|Bearer assertion, signed by IdP.|
|2|Bearer assertion, signed by IdP and encrypted to RP.|
|3|Holder of key assertion, signed by IdP and encrypted to RP.|

For example, FAL1 maps to the OpenID Connect Basic Client profile or SAML (Security Assertion Markup Language) Web SSO Artifact Binding profile, with no additional features. FAL2 additionally requires that the OpenID Connect ID Token or SAML Assertion be encrypted to a public key representing the RP in question. FAL3 requires the subscriber cryptographically prove possession of a key bound to the assertion (e.g., the use of a cryptographic authenticator) along with all requirements of FAL2. Note that the additional key presented at FAL3 need not be the same key used by the subscriber to authenticate to the IdP.

Regardless of what is requested or required by the protocol, the FAL in use is easily detected by the RP by observing the nature of the assertion as it is presented as part of the federation protocol. Therefore, the RP is responsible for determining which FALs it is willing to accept for a given authentication transaction and ensuring that the transaction meets the requirements of that FAL.

If the RP is using a front-channel presentation mechanism (e.g., the OpenID Connect Implicit Client profile or the SAML Web SSO profile), it SHOULD require FAL2 or greater in order to protect the information in the assertion from the browser or other parties in the transaction.

### 7.1. Key Management

At any FAL, the IdP SHALL ensure that an RP is unable to impersonate the IdP at another RP by protecting the assertion with a signature and key using approved cryptography. If the assertion is protected by a digital signature using an asymmetric key, the IdP MAY use the same public and private key pair to sign assertions to multiple RPs. The IdP MAY publish its public key in a verifiable fashion, such as at an HTTPS-protected URL at a well-known location. If the assertion is protected by a MAC using a shared key, the IdP SHALL use a different shared key for each RP.
