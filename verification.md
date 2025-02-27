# ✅ Issue and Verify Testing

* Version: 3.20.1
* Date: 2025-08-04 13:20:20
* Duration: 29s


## ✅ System Components

Components of the verifiable credentials system and their interconnections.

```mermaid
graph LR
    A[Holder] -- ✅ holder service has a valid certificate ⇄<br>✅ service version is 3.20.1 --> B(Issuer);
    C[Verifier]
    A -- ✅ verifier service has a valid certificate ⇄<br>✅ service version is 3.20.1 --> C;
    D[IdP]
    A -- ✅ idp service has a valid certificate ⇄<br>✅ service version is 3.20.1 --> D;
    B --> C;
    D --> B;
    D --> C;
```
    ✅ No network requests are made outside components
    ✅ All compenents are tested
     

## ✅ Complete Verifiable Credentials Workflow

A standard verifiable credential issuance, presentation, and verification flow with authentication via an Identity Provider. This flow ensures that a holder can successfully request and receive a credential from an issuer, present it to a verifier, and have it successfully verified, all while authenticating through an external IdP.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Holder->>IdP: Request Authentication ⇄
    Note right of IdP: ✅ click the button Login with IdP
    Note right of IdP: ✅ request json contains context "auth_token"
    Note right of IdP: ✅ request json format is JWT
    Note right of IdP: ✅ request json expiry is >5min
    Note right of IdP: ✅ request json issuer is "idp.example.com"
    IdP->>Holder: Provide Authentication Token ⇄
    Note right of IdP: ✅ see text Authentication Successful
    Note right of IdP: ✅ response json contains vc component "signature"
    Note right of IdP: ✅ response json algorithm is "ES256"
    Note right of IdP: ✅ response json user ID is "user123"
    Holder->>Issuer: Request Credential ⇄
    Note right of Issuer: ✅ click the button Request Credential
    Note right of Issuer: ✅ request json contains context "credential_request"
    Note right of Issuer: ✅ request json attributes are "name, dob"
    Note right of Issuer: ✅ request json schema is "https://example.com/credential.json"
    Issuer->>Holder: Issue Credential ⇄
    Note right of Issuer: ✅ see text Credential Issued Successfully
    Note right of Issuer: ✅ response json @context is "https://w3.org/2018/credentials/v1"
    Note right of Issuer: ✅ response json type is "VerifiableCredential"
    Note right of Issuer: ✅ response json issuer is "issuer.example.com"
    Note right of Issuer: ✅ response json credentialSubject is { id: "holder.example.com", name: "John Doe" }
    Holder->>Verifier: Present Credential ⇄
    Note right of Verifier: ✅ click the button Present Credential
    Note right of Verifier: ✅ request json signature is valid
    Note right of Verifier: ✅ request json credential ID is "vc-123"
    Verifier->>Holder: Verify Credential ⇄
    Note right of Verifier: ✅ see text Credential Verified Successfully
    Note right of Verifier: ✅ response json result is true
    Note right of Verifier: ✅ response json confidence is 0.95
```

## ✅ Unauthorized Access Tests

This diagram tests that the services cannot be accessed without authentication.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Holder->>Issuer: Request Credential without auth ⇄
    Note right of Issuer: ✅ Issuer returns error "Unauthorized"
```

## ✅ Non-valid Credentials Tests

This diagram tests that the services return an error when accessed with non-valid credentials.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Holder->>Issuer: Request Credential with non-valid credentials ⇄
    Note right of Issuer: ✅ Issuer returns error "non-valid credentials"
```

## ✅ Non-valid Requests Tests

This diagram tests that the services return an error when accessed with non-valid requests.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Holder->>Issuer: Request Credential with non-valid request ⇄
    Note right of Issuer: ✅ Issuer returns error "non-valid request"
```

## ✅ Non-valid Response JSON Tests

This diagram tests that the services return an error when the response json is non-valid.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Issuer->>Holder: Issue Credential with non-valid response json ⇄
    Note right of Issuer: ✅ Holder returns error "non-valid response json"
```

## ✅ Issue token with Non-valid request

This diagram tests that the services return an error after login where the credential is not valid.

```mermaid
sequenceDiagram
    participant Holder
    participant Issuer
    participant Verifier
    participant IdP

    Holder->>IdP: Request Authentication ⇄
    Note right of IdP: ✅ IdP returns valid Authentication Token
    IdP->>Holder: Provide Authentication Token ⇄
    Note right of Holder: ✅ Holder receives valid Authentication Token
    Holder->>Issuer: Request Credential ⇄
    Note right of Issuer: ✅ Issuer receives valid Credential Request
    Issuer->>Holder: Issue Credential with non-valid response json ⇄
    Note right of Issuer: ✅ Holder returns error "Non-valid response json"
```
