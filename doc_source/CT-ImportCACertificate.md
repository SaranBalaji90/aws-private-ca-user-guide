# Importing a Certificate Authority Certificate<a name="CT-ImportCACertificate"></a>

The following CloudTrail example shows the results of a call to the [ImportCertificateAuthorityCertificate](https://docs.aws.amazon.com/acm-pca/latest/APIReference/API_ImportCertificateAuthorityCertificate.html) operation\.

```
{
  eventVersion: "1.05",
  userIdentity: {
    type: "IAMUser",
    principalId: "account",
    arn: "arn:aws:iam::account:user/name",
    accountId: "account",
    accessKeyId: "Key_ID"
  },
  eventTime: "2018-01-26T21:53:28Z",
  eventSource: "acm-pca.amazonaws.com",
  eventName: "ImportCertificateAuthorityCertificate",
  awsRegion: "us-east-1",
  sourceIPAddress: "xx.xx.xx.xx",
  userAgent: "aws-cli/1.14.28 Python/2.7.9 Windows/8 botocore/1.8.32",
  requestParameters: {
    certificateAuthorityArn: "arn:aws:acm-pca:region:account:certificate-authority/ac5a7c2e-19c8-4258-b74e-351c2b791fe1",
    certificate: {
      hb: [45,
      45,
      ...10],
      offset: 0,
      isReadOnly: false,
      bigEndian: true,
      nativeByteOrder: false,
      mark: -1,
      position: 1257,
      limit: 1257,
      capacity: 1257,
      address: 0
    },
    certificateChain: {
      hb: [45,
      45,
      ...10],
      offset: 0,
      isReadOnly: false,
      bigEndian: true,
      nativeByteOrder: false,
      mark: -1,
      position: 1139,
      limit: 1139,
      capacity: 1139,
      address: 0
    }
  },
  responseElements: null,
  requestID: "36bbba0c-2d08-4995-99fc-964926103841",
  eventID: "17a38b26-49c1-41d2-8d15-f9362eeaaaa0",
  eventType: "AwsApiCall",
  recipientAccountId: "account"
}
```