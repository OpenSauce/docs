---
title: Disaster Recovery Overview
summary: Learn how to make your cluster resilient with high availability and disaster recovery features.
toc: true
---

Resilient deployments aim for continuity in database operation to protect from data loss and down time. To achieve resiliency, it is necessary to build deployments with high availability and disaster recovery coverage.

- [_High availability_](#choose-a-high-availability-strategy): Continuous and uninterrupted access to data even in the presence of failures or disruptions to maximize uptime.
- [_Disaster recovery_](#choose-a-disaster-recovery-strategy): Recovery from a major incident or disaster to minimize downtime and data loss.

CockroachDB provides resiliency for cluster deployments with [built-in replication]({% link {{ page.version.version }}/architecture/replication-layer.md %}) for high availability and disaster recovery features. Each workload running on a cluster will have different characteristics that will result in varying requirements for availability and disaster recovery.

{{site.data.alerts.callout_success}}
For a practical guide on how CockroachDB replicates, distributes, and rebalances data, refer to the [Replication and Rebalancing]({% link {{ page.version.version }}/demo-replication-and-rebalancing.md %}) demo.
{{site.data.alerts.end}}

## Resilience strategy

As you evaluate CockroachDB's disaster recovery features, it is essential to consider your organization's requirements for the amount of tolerable data loss and the acceptable length of time to recover.

- Recovery Point Objective (RPO): The maximum amount of time that an organization can tolerate losing data.
- Recovery Time Objective (RTO): The maximum length of time it should take to restore normal operations following an outage.

<img src="{{ 'images/v24.1/rpo-rto.png' | relative_url }}" alt="Simulating RPO and RTO. With RPO representing the tolerable data loss, and RTO representing the tolerable time to recovery." style="border:0px solid #eee;max-width:100%" />

For a comparison of CockroachDB resiliency features, refer to the following sections:

- [Choose a high availability strategy](#choose-a-high-availability-strategy)
- [Choose a disaster recovery strategy](#choose-a-disaster-recovery-strategy)

### Choose a high availability strategy

{% comment %}To add replicator as logical replication on public preview release{% endcomment %}

CockroachDB uses replication to create and distribute copies of data, ensuring consistency across the data copies. The database can tolerate nodes going offline without interrupting service whether in a single-region or [multi-region]({% link {{ page.version.version }}/multiregion-overview.md %}) cluster.

<table class="comparison-chart">
  <tr>
    <th></th>
    <th>Single-region replication (sync)</th>
    <th>Multi-region replication (sync)</th>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>RPO</b>
    </td>
    <td>0 seconds</td>
    <td>0 seconds</a></td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>RTO</b>
    </td>
    <td>Zero RTO + potential increased latency for 1-9 seconds (avg ~4.5s)</td>
    <td>Zero RTO + potential increased latency for 1-9 seconds (avg ~4.5s)</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Write latency</b>
    </td>
    <td>Region-local write latency, p50 latency < 5ms (for multiple availability zones in us-east1)</td>
    <td>Cross-region write latency, p50 latency > 50ms  (for a multi-region cluster in us-east1, us-east-2, us-west-1)</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Recovery</b>
    </td>
    <td>Automatic</td>
    <td>Automatic</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Hardware cost</b>
    </td>
    <td>1x + availability zone to availability zone network</td>
    <td>1x + datacenter to datacenter network</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Minimum regions</b>
    </td>
    <td>1</td>
    <td>3</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Fault tolerance</b>
    </td>
    <td>Zero RPO node, availability zone failures</td>
    <td>Zero RPO node, availability zone failures</td>
  </tr>

</table>

For detail on designing your cluster topology for high availability with replication, refer to the [Disaster Recovery Planning]({% link {{ page.version.version }}/disaster-recovery-planning.md %}#hardware-failure) page.

### Choose a disaster recovery strategy

CockroachDB is designed to recover automatically, however building [backups]({% link {{ page.version.version }}/backup-and-restore-overview.md %}) or [physical cluster replication]({% link {{ page.version.version }}/physical-cluster-replication-overview.md %}) into your disaster recovery planning is an important part of a resilient deployment.

<table class="comparison-chart">
  <tr>
    <th></th>
    <th>Point in time backup & restore</th>
    <th>Physical cluster replication (async)</th>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>RPO</b>
    </td>
    <td>>=5 minutes</td>
    <td>~10s of seconds</a></td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>RTO</b>
    </td>
    <td>~ minutes to hours, depending on data size and number of nodes</td>
    <td>~ seconds to minutes, depending on cluster size</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Write latency</b>
    </td>
    <td>No impact</td>
    <td>No impact</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Recovery</b>
    </td>
    <td>Manual restore</td>
    <td>Manual failover</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Hardware cost</b>
    </td>
    <td>1x + S3-like storage & networking</td>
    <td>2x + datacenter to datacenter networking</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Minimum regions</b>
    </td>
    <td>1</td>
    <td>2</td>
  </tr>

  <tr>
    <td class="comparison-chart__feature">
      <b>Fault tolerance</b>
    </td>
    <td>Not applicable</td>
    <td>Zero RPO node, availability zone region failure with loss up to RPO</td>
  </tr>

</table>
