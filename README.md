# Multi-Region Resilience Platform

A production-grade, multi-region AWS platform — built with Terraform, GitOps (ArgoCD), and a set of custom Go tools for actually testing the resilience claims the architecture makes, not just assuming them.

## What's here

**Infrastructure (Terraform)**
- Multi-region EKS — Fargate hosts bootstrap compute (ArgoCD, core cluster add-ons) since Karpenter needs somewhere to run before it can provision anything; Karpenter provisions real EC2 nodes for everything else
- VPC, IAM/OIDC, KMS per region
- AWS Global Accelerator + Route53 for a single global entry point across regions
- DynamoDB Global Table and Aurora Global Database modules (no call-site yet — waiting on a real application schema to justify one)

**GitOps (ArgoCD)**
- App-of-apps pattern, one independent ArgoCD instance per region
- Karpenter, AWS Load Balancer Controller, ExternalDNS, and a demo app, all deployed the same way

**Tooling (Go)** — `tools/`
- [`platformctl`](tools/platformctl) — a CLI wrapping this repo's own Terraform stack layout (bootstrap / global / region), so applying, planning, and destroying any stack is one consistent command instead of remembering backend config per directory
- [`chaos`](tools/chaos) — a chaos engineering CLI: kills pods, or cordons/evicts real nodes (by count or by availability zone) to prove the multi-region architecture actually survives what it claims to
- [`probe`](tools/probe) — checks whether regional endpoints are actually up, and measures real outage-recovery time when paired with `chaos` — the number behind a resilience claim, not a guess

## Status

Observability (Prometheus/Grafana/Alertmanager) is built but currently paused, not deployed. Everything else above is real, applied infrastructure.

## Architecture

(coming soon)

## Deployment

(coming soon)
