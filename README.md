# verify-pdf

This is a fork of https://github.com/ninja-labs-tech/verify-pdf

verify pdf files in JS (supports node.js).

## Verifying PDF signature

The signed PDF file has the public certificate embedded in it, so all we need to verify a PDF file is the file itself.

## Installation

```
npm i @jonz94/verify-pdf
```

## Importing

```javascript
// CommonJS require
const verifyPDF = require('@jonz94/verify-pdf');

// ES6 imports
import verifyPDF from '@jonz94/verify-pdf';
```

## Verifying

Verify the digital signature of the pdf and extract the certificates details

```javascript
const verifyPDF = require('@jonz94/verify-pdf');
const signedPdfBuffer = fs.readFileSync('yourPdf');

const {
    verified,
    authenticity,
    integrity,
    expired,
    signatures
} = verifyPDF(signedPdfBuffer);
```

* signedPdfBuffer: signed PDF as buffer.
* verified: The overall status of verification process.
* authenticity: Indicates if the validity of the certificate chain and the root CA (overall in case of multiple signatures).
* integrity: Indicates if the pdf has been tampered with or not (overall in case of multiple signatures).
* expired: Indicates if any of the certificates has expired.
* signatures: Array that contains the certificate details and signatureMeta (Reason, ContactInfo, Location and Name) for each signature.

## Certificates

You can get the details of the certificate chain by using the following api.

```javascript
const { getCertificatesInfoFromPDF } = require('@jonz94/verify-pdf');  // require

import { getCertificatesInfoFromPDF } from '@jonz94/verify-pdf';  // ES6
```

```javascript
const certs = getCertificatesInfoFromPDF(signedPdfBuffer);
```

* signedPdfBuffer: signed PDF as buffer.

* certs:

    * issuedBy: The issuer of the certificate.
    * issuedTo: The owner of the certificate.
    * validityPeriod: The start and end date of the certificate.
    * pemCertificate: Certificate in pem format.
    * clientCertificate: true for the client certificate.

## Dependencies

[node-forge](https://github.com/digitalbazaar/forge) is used for working with signatures.

## Credits

* The initial implementation https://github.com/ninja-labs-tech/verify-pdf

* The process of signing and verifying a document is described in the [Digital Signatures in PDF](https://www.adobe.com/devnet-docs/acrobatetk/tools/DigSigDC/Acrobat_DigitalSignatures_in_PDF.pdf) document.
* This incredible [Stack Overflow answer](https://stackoverflow.com/questions/15969733/verify-pkcs7-pem-signature-unpack-data-in-node-js/16148331#16148331) for describing the whole process of verifying PKCS7 signatures.
