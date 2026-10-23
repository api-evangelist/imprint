---
title: "Debugging Spinnaker Connectivity Issues with EKS Clusters Across VPCs: A Journey Through AWS Networking"
url: "https://tech.imprint.co/debugging-spinnaker-connectivity/"
date: "2024-11-15"
feed_url: "https://tech.imprint.co/index.xml"
---
In a recent debugging journey, we encountered a puzzling connectivity issue: our deployment tool, Spinnaker, which operates in a backend EKS cluster, suddenly lost access to resources in another cluster used for our public gateway. With recent EKS upgrades underway, we initially suspected a security group misconfiguration. However, this wasn’t a typical connectivity issue as no recent changes could clearly explain it.
