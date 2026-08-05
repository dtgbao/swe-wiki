---
title: "Amazon EC2 instance types"
kind: system
domain: aws/ec2
status: draft
tags: [swe, aws, ec2, instance-types, compute, memory, storage, accelerators, hpc, sizing, performance, cost]
sources: ["sources/2026-07-30-ec2-instance-types-basics.md", "https://aws.amazon.com/ec2/instance-types/", "https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html", "https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-type-names.html", "https://docs.aws.amazon.com/ec2/latest/instancetypes/gp.html", "https://docs.aws.amazon.com/ec2/latest/instancetypes/co.html", "https://docs.aws.amazon.com/ec2/latest/instancetypes/mo.html", "https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-lifetime.html", "https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-free-tier-usage.html", "https://docs.aws.amazon.com/compute-optimizer/latest/ug/view-ec2-recommendations.html"]
updated: 2026-07-30
confidence: medium
---

# Amazon EC2 instance types

An Amazon EC2 instance type is a named hardware-resource configuration selected to match a workload's compute, memory, accelerator, storage, and network requirements.

Instance types are grouped into families with related characteristics. Category
and family names narrow the search; the exact instance type, Region, workload
behavior, performance evidence, and total cost determine the production choice.

## Naming model

AWS names an instance type as an instance family followed by a period and an
instance size.

Using the course example:

| Segment | `m5.2xlarge` meaning |
| --- | --- |
| `m` | Series; `M` is general purpose |
| `5` | Generation within the series |
| `2xlarge` | Size within the family |

The complete model also permits option letters within the family. In
`c7gn.xlarge`, for example, `c` is the compute-optimized series, `7` is the
generation, `g` identifies AWS Graviton processors, `n` identifies a network-
and-EBS-optimized variant, and `xlarge` is the size. `metal` is a possible
bare-metal size.

Do not add `.instance` to an instance-type identifier. Do not assume that all
families scale in identical ratios: look up the exact vCPU, memory, processor
architecture, network, EBS, accelerator, and instance-store specification.

Source: [AWS instance-type naming conventions](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-type-names.html).

## Workload categories

AWS's instance-types landing page lists these six categories as of 2026-07-30:

| Category | Selection signal | Representative workloads |
| --- | --- | --- |
| General purpose | Compute, memory, and networking are needed in broadly balanced proportions | Web services, code repositories, small-to-medium databases |
| Compute optimized | CPU is the dominant constraint | Batch processing, media transcoding, high-throughput web serving, game servers |
| Memory optimized | Large in-memory working sets or high memory-to-vCPU ratios dominate | In-memory databases, analytics, enterprise applications |
| Accelerated computing | GPUs, FPGAs, inference or training chips, or other coprocessors materially accelerate the work | Graphics, floating-point computation, inference, training, pattern matching |
| Storage optimized | High sequential throughput or low-latency random I/O against large local data sets dominates | High-throughput databases, data processing, data streaming |
| HPC optimized | Tightly coupled high-performance computing needs price-performance at scale | Complex simulation, deep learning, visual-effects rendering |

Family membership and available generations change over time. Use the
[current instance-type catalog](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html)
instead of encoding a slide's family list as a durable invariant.

## Selection workflow

1. **Profile the workload.** Identify the limiting resource and its shape:
   sustained or burst CPU, memory working set, accelerator requirements, local
   IOPS or throughput, network traffic, EBS traffic, and latency sensitivity.
2. **Filter for compatibility.** Check processor architecture, AMI and
   operating-system support, virtualization and accelerator requirements,
   licensing, and Region or Availability Zone availability.
3. **Choose candidate families and options.** Use a category as the entry point,
   then account for option letters such as processor, local instance store,
   network, EBS, extra capacity, or high-frequency variants.
4. **Compare exact types.** Inspect vCPUs, GiB of memory, baseline and burst
   behavior, network bandwidth, EBS bandwidth and IOPS, instance-store devices,
   security features, and quotas at the exact size.
5. **Model total cost.** Include the selected purchase model, operating-system
   charges, attached storage, data transfer, accelerators, and software
   licensing—not only the instance's headline hourly price.
6. **Benchmark and right-size.** Test a representative load, retain headroom
   for reliability, observe production utilization, and revisit the choice as
   workload behavior, generations, prices, and availability change. AWS
   Compute Optimizer can provide evidence-based candidates, but its
   recommendations still require workload validation.

This workflow turns an instance family into a hypothesis to test rather than a
permanent architecture decision.

## Performance modes and resource boundaries

- Fixed-performance families provide fixed CPU resources.
- Burstable `T` families provide a CPU baseline and spend CPU credits to burst
  above it. They fit spiky general-purpose workloads but can behave differently
  under sustained load.
- Some host resources are dedicated to an instance while others, including
  parts of the network and disk subsystem, can be shared. Published baseline,
  burst, and maximum values matter.
- Network performance, EBS performance, and local instance-store performance
  are separate dimensions. A vCPU and memory comparison alone is insufficient.

## Local instance-store durability boundary

Storage-optimized instances are designed around high-performance access to
large local data sets, but local instance store is ephemeral:

- Data survives an instance reboot.
- Data does not survive stop, hibernation, termination, an instance-type
  change, or several host-recovery events.
- Durable state must be replicated or copied to persistent storage such as
  EBS, S3, or EFS.

Therefore, storage-optimized database, cache, warehouse, and distributed-file-
system designs need an explicit replication, recovery, and backup strategy.
The instance family changes performance characteristics; it does not supply
durability by itself.

Source: [AWS instance-store persistence](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-lifetime.html).

## Course comparison, corrected

| Instance type | vCPUs | Memory | Interpretation |
| --- | ---: | ---: | --- |
| `t2.micro` | 1 | 1 GiB | Burstable general purpose |
| `m5.2xlarge` | 8 | 32 GiB | General purpose |
| `r5.16xlarge` | 64 | 512 GiB | Memory optimized; the course transcript incorrectly says 16 vCPUs |
| `c5d.4xlarge` | 16 | 32 GiB | Compute optimized with local instance store; the transcript spells it `c5.d.4xlarge` |

The values above are family specifications, not current purchase
recommendations. Availability, generation status, price, and Free Tier
eligibility are mutable and must be checked when deploying.

Sources: [general-purpose specifications](https://docs.aws.amazon.com/ec2/latest/instancetypes/gp.html),
[compute-optimized specifications](https://docs.aws.amazon.com/ec2/latest/instancetypes/co.html),
and [memory-optimized specifications](https://docs.aws.amazon.com/ec2/latest/instancetypes/mo.html).

## Free Tier boundary

Do not encode `t2.micro` as the universal Free Tier choice. AWS documents
different eligible instance-type lists and program behavior for accounts
created before 2025-07-15 versus accounts created on or after that date. Query
current eligibility or inspect the launch workflow for the target account and
Region.

Source: [AWS EC2 Free Tier usage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-free-tier-usage.html).

## Failure modes

- Choosing from a memorized family list that is stale or unavailable in the
  target Region.
- Treating a category as proof of performance without comparing the exact
  size, options, and baseline or burst limits.
- Moving to an incompatible processor architecture without validating the AMI,
  native dependencies, or software licensing.
- Confusing instance store with EBS or assuming local high-performance storage
  is durable.
- Optimizing only CPU and memory while EBS, network, accelerator, or I/O limits
  remain the bottleneck.
- Selecting from list price alone without measuring representative workload
  performance and total system cost.
- Treating a right-sizing recommendation as self-validating instead of testing
  performance risk and required headroom.

## Related

- [EC2 Instance Types Basics source](sources/2026-07-30-ec2-instance-types-basics.md)
- [AWS global infrastructure and service scope](../foundations/aws-global-infrastructure.md)
- [AWS EC2 instance-types catalog](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html)
- [AWS Compute Optimizer EC2 recommendations](https://docs.aws.amazon.com/compute-optimizer/latest/ug/view-ec2-recommendations.html)
