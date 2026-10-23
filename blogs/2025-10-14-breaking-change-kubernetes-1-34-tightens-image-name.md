---
title: "Breaking Change: Kubernetes 1.34 Tightens Image Name Resolution Rules — What It Means for You"
url: "https://www.replicated.com/blog/breaking-change-kubernetes-1-34-tightens-image-name-resolution-rules----what-it-means-for-you"
date: "2025-10-14"
feed_url: "https://www.replicated.com/blog/rss.xml"
---
Kubernetes 1.34 introduces a breaking change for clusters using the CRI-O runtime, such as OpenShift and Oracle OKE — unqualified image names will no longer resolve automatically. This update enhances supply chain security but may cause deployment failures for manifests or Helm charts using short image names. In this post, Replicated explains what changed, why it matters, and how teams can quickly adapt.
