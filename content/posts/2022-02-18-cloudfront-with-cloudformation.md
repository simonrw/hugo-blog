---
title: "Deploying CloudFront with CloudFormation"
date: 2022-02-18
tags:
  - aws
  - cloudfront
  - cloudformation
summary: A quick gotcha on deploying CloudFront following best practices
---

**TL;DR: don't forget about the default cache policy**

I was trying to deploy a CloudFront distribution with CloudFormation. I wanted to follow best practices, so I was trying
to use a cache policy rather than embedding the cache configuration inline with the `ForwardedValues` field.

I had my cache behaviours defined:

```yaml
Resources:
  Distribution:
    Type: AWS::CloudFront::Distribution
    Properties:
      # ...
      CacheBehaviors:
        - CachePolicyId: !Ref CachePolicy

  CachePolicy:
    Type: AWS::CloudFormation::CachePolicy
    Properties:
      # ...
```

I was deploying and getting the error "The parameter ForwardedValues is required". I was bashing my head against this,
trying to use `{}` or `!Ref AWS::NoValue` but that didn't fix things.

I wanted to follow best practices, and defining a cache policy is more powerful.

Finally my googling uncovered the answer:

...I had forgotten to include the `CachePolicyId` field in by `DefaultCachePolicy`!

---

One more quick thing - as a brit, I slipped up on typing `Behaviours` (extra "u") all the time. Thanks to `cfn-lint` and
the CloudFormation tools for helping me catch that early!
