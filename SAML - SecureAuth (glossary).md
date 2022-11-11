---
title: SAML - SecureAuth (Glossary)
tags:
  - SAML
  - infosec
---

Key terms and concepts [SecureAuth](https://docs.secureauth.com/2104/en/saml-application-integration.html) :

1) Connection Type
	- **SP Initiated** – Starts the login process at the service provider / application, then redirects the user to the Identity Platform for authentication, and upon successful authentication, it finally asserts the user back to the application. The SAML specification allows for service providers to send authentication requests (AuthnRequest) to the Identity Platform either by HTTP Redirect or HTTP Post. The service provider configuration or metadata tells you what is used for the authentication request.
		- **By Redirect** – Use HTTP Redirect binding to send the AuthnRequest with the signature related to the request.
		- **By Post** – Use HTTP Post binding to send the AuthnRequest with the embedded signature.
	- **IdP Initiated** – Starts the login process at the Identity Platform, and upon successful authentication, asserts the user to the application.

2) Assertion
	- **IdP Issuer** (WSFedSAML Issuer) : A unique name that must match exactly on the Identity Platform side and the application side. This helps the SAML application identify the Identity Platform as the SAML issuer.
	- **Assertion Consumer Service (ACS)** (SAML Consumer URL): Enter the URL from the service provider to let the application accept a SAML assertion from the Identity Platform.
	- **Relay State** : Optional. User is directed to this URL after authentication.
	- **Recipient** : Optional. This field is usually the same as the consumer (ACS) URL.
	- **Audience** : Optional. A unique string that identifies the service provider (SP). Usually, this is the entity ID of the service provider.
	- **SP Login URL** : This is the login URL of the Service Provider (SP). Usually, this is the same address the Relay State or Assertion Consumer Service (ACS) URL.
	- **Assertion will be valid for** : Indicate in **hours** and **minutes**, how long the SAML assertion is valid. It is referred to as SAML NotOnOrAfter in the SAML Specifications. The default setting is one hour, but for more sensitive application resources, the recommended value is between one to five minutes.
	- **Offset Minutes** : Indicate in **minutes** to account for the time differences among devices. This is referred to as SAML NotBefore in the SAML Specifications. Recommended value is five minutes.
	- **IdP Signing Certificate** : Click **Select Certificate**, choose the IdP signing certificate to use, and then click Select to close the box.
	- **IdP Signing Certificate Serial Number** : When you select an IdP signing certificate, the serial number populates this field.
	- **Signing Algorithm** : The signing algorithm digitally signs the SAML assertion and response. Choose the signing algorithm – **SHA1** or **SHA2** (slightly stronger encryption hash and is not subject to the same vulnerabilities as SHA1).
	- **Sign SAML Assertion** : Enable signing of the SAML assertion to ensure assertion integrity when the assertion is delivered to the service provider (SP).
	- **Sign SAML Message** : Enable signing of the SAML message to ensure message integrity when the message is delivered to the service provider (SP).
	- **Encrypt SAML Assertion** : Enable encryption of the SAML assertion if only the service provider and the Identity Platform should understand the assertion. Next, select the data and key encryption methods:
		- **Data Encryption Method** – Select the algorithm of the data encryption method
	    - **Key Encryption Method** – Select the type of key encryption method (symmetric or asymmetric)

