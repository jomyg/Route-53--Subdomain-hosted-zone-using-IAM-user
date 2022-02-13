# Route-53-Subdomain-level-hosted-zone-using-IAM-user
Allowing access to specific hosted zones with Route 53 and IAM 
Route 53 - Hosted zone subdomain level restriction using IAM POLICY

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)]()

## Description:
Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. It is designed to give developers and businesses an extremely reliable and cost effective way to route end users to Internet applications by translating names like www.jomygeorge.com into the numeric IP addresses like 192.0.2.1 that computers use to connect to each other. Amazon Route 53 is fully compliant with IPv6 as well.

A hosted zone is an Amazon Route 53 concept. A hosted zone is analogous to a traditional DNS zone file; it represents a collection of records that can be managed together, belonging to a single parent domain name. All resource record sets within a hosted zone must have the hosted zone's domain name as a suffix.

## Pre-requisites:

1) Route 53 with two seprate hosted zone, one with main domain jomygeorge.xyz and other hosted zone with subdomain tech.jomygeorge.xyz. Add the subdomain Nameservers on main domain record filed  as a NS record using simple routing within the route 53.
2) IAM User with coustom policy for only access to subdomain hosted zone not main domain. Inorder to create IAM user

```
Go to IAM users console -> Create user "ALEX" Using console access setup
```
## Policy one
### Policy i have created for the ALEX user to manage the tech.jomygeorge.xyz subdomain using IAM :
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ListResourceRecordSets",
                "route53:ListHostedZones",
                "route53:ChangeResourceRecordSets",
                "route53:ListResourceRecordSets",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZonesByName"
            ],
            "Resource": "arn:aws:route53:::hostedzone/<Hosted zone ID of subdomain from route 53>"
        }
    ]
}
```
#### IAM user can access the route 53 console by calling the below link:
```sh
https://console.aws.amazon.com/route53/home?#resource-record-sets:<Hosted-zone-ID-of-subdomain-from-route-53>
```

## Policy two 
### Below policy can also use, so that user can access route 53 main dashboard without calling above URL:
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ChangeResourceRecordSets",
                "route53:ListResourceRecordSets"
            ],
            "Resource": "arn:aws:route53:::hostedzone/Z0064862302RH0WZLAWCL"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "route53:ListHostedZones",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZonesByName"
            ],
            "Resource": "*"
        }
    ]
}
```

## Conclusion:

IAM User Alex can now access route 53 for manage his subdomain hosted zone without accessing the main domain
