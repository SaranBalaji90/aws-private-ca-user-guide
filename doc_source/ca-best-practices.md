# ACM Private CA Best Practices<a name="ca-best-practices"></a>

Best practices are recommendations that can help you use ACM Private CA effectively\. The following best practices are based on real\-world experience from current AWS Certificate Manager and ACM Private CA customers\. 

## Documenting CA Structure and Policies<a name="document-ca"></a>

AWS recommends documenting all of your policies and practices for operating your CA\. This might include:
+ Reasoning for your decisions about CA structure
+ A diagram showing your CAs and their relationships
+ Policies on CA validity periods
+ Planning for CA succession
+ Policies on path length
+ Catalog of permissions
+ Description of administrative control structures
+ Security

You can capture this information in two documents, known as Certification Policy \(CP\) and Certification Practices Statement \(CPS\)\. Refer to [RFC 3647](https://www.ietf.org/rfc/rfc3647.txt) for a framework for capturing important information about your CA operations\.

## Minimize Use of the Root CA<a name="minimize-root-use"></a>

Issuing end\-entity certificates from a root CA 

Issuing end\-entity certificates directly from a root CA is **not****recommended, except in exceptional circumstances\. **ACM Private CA makes it possible to issue end\-entity certificates from your root CA\. AWS recommends creating a subordinate issuing CA from which to issue end\-entity certificates\. The only exception to this rule is if you are replacing a software\-based root issuing CA with a simple, cost\-effective ACM Private CA root issuing CA, and you are willing to accept the limitations of this configuration\. This configuration imposes limitations that may result in operational challenges later\. For example, if the root CA is compromised or lost, you must create a new root CA and distribute it to the clients in your environment\. During this time and until you create and distribute a new root CA, you will not be able to issue new certificates\. Issuing certificates directly from a Root CA also prevents you from restricting access and limiting the number of certificates issued from your root, which are both considered best practices for managing a root CA\. If your current process is to create a software\-based root CA with a private key in memory or stored on a disk, and you issue end\-entity certificates directly from this root CA, then using ACM Private CA to create a root CA and issuing end\-entity certificates from the root would be advantageous\. For example, if you have a software\-based CA that you created in OpenSSL, with the root CA private key stored on disk, ACM Private CA provides better security and operational controls\. To issue end\-entity certificates from your root CA, you must also have an IAM permissions policy that permits you to use the end\-entity certificate template with your root CA \[Link to IAM policy section\]

## Give the Root CA its own AWS Account<a name="isolate-root-account"></a>

Creating a root CA and subordinate CA in two different AWS accounts is a recommended best practice\. Doing so can provide you with additional protection and access controls for your root CA\. You can do so by exporting the CSR from the subordinate CA in one account, and signing it with a root CA in a different account\. The benefit of this approach is that you can separate control of your CAs by account\. The disadvantage is that you cannot use the AWS management console wizard to simplify the process of signing the CA certificate of a subordinate CA from your Root CA\.

## Turn on AWS CloudTrail<a name="use-cloudtrail"></a>

Turn on CloudTrail logging before you create and start operating a private CA\. With CloudTrail you can retrieve a history of AWS API calls for your account to monitor your AWS deployments\. This history includes API calls made from the AWS Management Console, the AWS SDKs, the AWS Command Line Interface, and higher\-level AWS services\. You can also identify which users and accounts called the PCA API operations, the source IP address the calls were made from, and when the calls occurred\. You can integrate CloudTrail into applications using the API, automate trail creation for your organization, check the status of your trails, and control how administrators turn CloudTrail logging on and off\. For more information, see [Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)\. Go to [Using CloudTrail](PcaCtIntro.md) to see example trails for ACM Private CA operations\. 

## Rotate the CA Private Key<a name="rotate-keys"></a>

It is a best practice to periodically update the private key for your private CA\. You can update a key by importing a new CA certificate, or you can replace the private CA with a new CA\.

## Delete an Unused CA<a name="delete-unused-ca"></a>

You can permanently delete a private CA\. You might want to do so if you no longer need the CA or if you want to replace it with a CA that has a newer private key\. To safely delete a CA, we recommend that you follow the process outlined in [Deleting Your Private CA](PCADeleteCA.md)\.

## Redundancy and Disaster Recovery<a name="disaster-recovery"></a>

Creating and distributing two roots/hierarchies for DR

Consider redundancy and DR when planning your CA hierarchy\. ACM Private CA is available in multiple regions \[link to region availability\], which allows you to create redundant CAs in multiple regions\. The ACM Private CA service operates with a [service level agreement](https://aws.amazon.com/certificate-manager/private-certificate-authority/sla/) \(SLA\) of 99\.9% availability\. There are at least two approaches you can consider for redundancy and disaster recovery\. You can configure redundancy at the Root CA or at the highest subordinate CA\. Each approach has pros and cons\. 

1. You can create two root CAs in two different AWS Regions for redundancy and disaster recovery\. With this configuration, each root CA operates independently in an AWS Region, protecting you in the event of a single\-region disaster\. Creating redundant root CAs does, however, increase operational complexity — you will need to distribute both root CA certificates to the trust stores of browsers and operating systems in your environment\. 

1. You can also create two redundant subordinate CAs that chain to the same root CA\. The benefit of this approach is that you need to distribute only a single root CA certificate to the trust stores in your environment\. The limitation is that you don’t have a redundant root CA in the event of a disaster that affects the AWS Region in which your root CA exists\.