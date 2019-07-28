---
layout: post
title:  "Be careful with AWS Private API Gateway Endpoints"
date:   2019-07-24 18:00:00 +0200
tags: [aws, rant]
imagefeature: 2019-07-aws-api-gateway/aws-api-gateway-icon.png
description: Don't kill your production systems!

comments: true
share: true

---

As many shops out there, we (at [EMnify](https://www.emnify.com)) are using AWS Lambda for an increasing number of applications to make our life easier.
The prerequisite to trigger Lambda functions via REST API calls is to deploy an API Gateway into an AWS account.

Historically, API Gateway offered deployment types: 

- _regional_: what I would consider expected - the API is reachable directly via a DNS host name including the AWS region where it is deployed
- _edge-optimized_: what feels weird and almost useless - the API Gateway automatically deploys a somewhat hidden CloudFront distribution, which is not further configurable for the user; I would recommend to use _regional_ deployment and set up the CF distribution yourself instead, if the CloudFront CDN should be used.

A bit more than one year ago, AWS has launched a third type:

- [_private_](https://aws.amazon.com/blogs/compute/introducing-amazon-api-gateway-private-endpoints/): **what I consider dangerous** - the API is only reachable via a VPC endpoint inside your VPC.

This harsh statement might come unexpected, as most readers might be actually fans of VPC endpoints and so am I. But in this case, the setting _Enable Private DNS Name_, which I would certainly want to activate, **will prevent all access to any regional/public API Gateway in the whole AWS region!**

Assuming DNS host names are enabled in the VPC configuration, this setting for the VPC endpoint changes the DNS resolution for API Gateway host names (`<api-id>.execute-api.<region>.amazonaws.com`) to return the IP addresses of the VPC endpoint's ENIs. So far, this is expected and in line with how all other VPC Interface Endpoints work (like `kms.eu-west-1.amazonaws.com` then pointing to private IPs). The caveat however is that **any request to an API Gateways in the whole region will** pass through the VPC endpoint because of one of its DNS entries is `*.execute-api.<region>.amazonaws.com`, which will very unexpectedly **be rejected with status `403 Forbidden`**.

To recap: all calls to API Gateways located in the same AWS region accessed from the whole VPC will return status 403. This caused us headaches, as this problem stayed first unnoticed in our infrastructure and - of course - was not detected when testing the private API Gateway.

So what are the alternatives? One can - and in my opinion has to, unless you can ensure that forever nobody will call a public API from this VPC - disable the private DNS name. Then accessing the private API, however, becomes a lot more complicated.

The documentation [Create a Private API in Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-private-apis.html) as of now (July 2019) reads pretty fine, just as like you use the VPC endpoint for accessing the API. But what does this tiny `Host: ...` line mean?

![private API Gateway](/images/2019-07-aws-api-gateway/apigateway-private-api-accessing-api.png)

Reading the page three times still kept me confident I'm doing the right thing.

More clear is this FAQ entry: [Why can't I connect to my public API from an API Gateway VPC endpoint?](https://aws.amazon.com/premiumsupport/knowledge-center/api-gateway-vpc-connections/). This clarifies that one of the two following HTTP headers must me set:

```
Host: <api-id>.execute-api.<region>.amazonaws.com
```
or

```
x-apigw-api-id: <api-id>
```

when sending the HTTP request to the hostname of the VPC endpoint (`https://<vpce-id>.execute-api.<region>.vpce.amazonaws.com/stageName`).
This requires you to have pretty deep control over the applications accessing the private API. And you need to be willing to modify them accordingly.

Another alternative, as I was told by AWS Community Hero [Thorsten Höger](https://www.taimos.de/), which would not require to set custom headers with disabled DNS resolution, would be based on a custom domain name configured in the API Gateway and an ALB in front of it. Based on an IP target group, the ALB would forwards the requests to the ENIs of the VPC Endpoint. Looks like an option to move the complexity into the network.

So far, it became not at all clear to me (and AWS support was not able to explain), why all calls to public APIs are rejected. If this would work the execute-api VPC endpoint would behave like any other - and not kill your setup. #awswishlist

If my understanding is wrong or if you have a better solution, I'm happy to read from you in the comments below! 