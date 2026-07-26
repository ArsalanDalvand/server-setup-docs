---
title: "Redis Software product lifecycle"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/product-lifecycle/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/product-lifecycle"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Software product lifecycle"
---
# Redis Software product lifecycle

The product lifecycle of Redis Software cluster versions.

            Redis Software
          

The Redis Software product lifecycle fully reflects the [subscription agreement](https://redis.io/legal/software-agreement/).
However, for any discrepancy between the two policies, the subscription agreement prevails.

Redis Software modules follow the [modules lifecycle](/docs/latest/operate/oss_and_stack/stack-with-enterprise/modules-lifecycle/).

## Redis Software cluster version lifecycle

This section describes the lifecycle policy for Redis Software cluster versions.

### Cluster version release numbers

Redis Software uses a four-place numbering scheme to designate released cluster versions.
The format is "Major1.Major2.Minor-Build".

Major sections of the version number represents fundamental changes and additions in
capabilities to Redis Software. The Major1 and Major2 part of the
version number are incremented based on the size and scale of the changes in each
release.
The Minor section of the version number represents quality improvements, fixes to
existing capabilities, and new capabilities which are typically minor, feature-flagged, or optional.
Build number is incremented with any changes to the product. Build number is
incremented with each build when any change is made to the binaries.

Redis Software typically gets two major releases every year but the product shipping cycles may vary.
Maintenance releases, typically available on the last minor release of the current major1.major2 release are typically made available on a monthly cadence, although cycles may vary.

### Cluster version end-of-life schedule

For Redis Software cluster versions 6.2 and later, the end-of-life (EOL) for each major release occurs 24 months after the formal release of the subsequent major version. Monthly maintenance will be provided on the last minor release of the major1.major2 releases.
This update to the EOL policy allows a lead time of at least 24 months to upgrade to the new cluster version after it is available.

Version - Release date
End of Life (EOL)

8.2 â July 2026
-

8.0 â October 2025
July 31, 2028

7.22 â May 2025
October 30, 2027

7.8 â November 2024
May 30, 2027

7.4 â February 2024
November 30, 2026

7.2 â August 2023
February 28, 2026

6.4 â February 2023
August 31, 2025

6.2 â August 2021
February 28, 2025

6.0 â May 2020
May 31, 2022

5.6 â April 2020
October 31, 2021

5.4 â December 2018
December 31, 2020

5.2 â June 2018
December 31, 2019

The following timeline chart visualizes the Redis Software product lifecycle, showing release dates and end-of-life dates for each major version:

```
8.2: Jul 2026 - TBD
8.0: Oct 2025 - Jul 31, 2028
7.22: May 2025 - Oct 30, 2027
7.8: Nov 2024 - May 30, 2027
7.4: Feb 2024 - Nov 30, 2026
7.2: Aug 2023 - Feb 28, 2026
6.4: Feb 2023 - Aug 31, 2025
6.2: Aug 2021 - Feb 28, 2025
6.0: May 2020 - May 31, 2022
5.6: Apr 2020 - Oct 31, 2021
5.4: Dec 2018 - Dec 31, 2020
5.2: June 2018 - Dec 31, 2019
```

#### Redis Software product lifecycle

```
8.2: Jul 2026 - TBD
8.0: Oct 2025 - Jul 31, 2028
7.22: May 2025 - Oct 30, 2027
7.8: Nov 2024 - May 30, 2027
7.4: Feb 2024 - Nov 30, 2026
7.2: Aug 2023 - Feb 28, 2026
6.4: Feb 2023 - Aug 31, 2025
6.2: Aug 2021 - Feb 28, 2025
6.0: May 2020 - May 31, 2022
5.6: Apr 2020 - Oct 31, 2021
5.4: Dec 2018 - Dec 31, 2020
5.2: June 2018 - Dec 31, 2019
```

### Supported cluster version upgrade paths

â Supported â You can upgrade directly from the current Redis Software cluster version.

❌ Not supported â You cannot upgrade directly from the current Redis Software cluster version. You must first upgrade to a supported intermediate version.

Current Redis Software cluster version
Upgrade to Redis Software 7.2.x
Upgrade to Redis Software 7.4.x
Upgrade to Redis Software 7.8.x
Upgrade to Redis Software 7.22.x
Upgrade to Redis Software 8.0.2-8.0.10
Upgrade to Redis Software 8.0.16-8.0.20
Upgrade to Redis Software 8.2.x

6.0.x
â
❌
❌
❌
❌
❌
❌

6.2.4
6.2.8
â
â
❌
❌
❌
❌
❌

6.2.10
6.2.12
6.2.18
â
â
â
❌
❌
❌
❌

6.4.x
â
â
â
â
â
❌
❌

7.2.x
â
â
â
â
â
â
â

7.4.x
â
â
â
â
â
â
â

7.8.x
â
â
â
â
â
â
â

7.22.x
â
â
â
â
â
â
â

8.0.2
8.0.6
8.0.10
â
â
â
â
â
â
â

8.0.16
8.0.18
8.0.20
â
â
â
â
-
â
â

8.2.x
â
â
â
â
â
â
â

For detailed upgrade instructions, see [[operate/rs/installing-upgrading/upgrading/upgrade-cluster|Upgrade a Redis Software cluster]].

Note:
  Redis Enterprise for Kubernetes has its own support lifecycle, which accounts for the Kubernetes distribution lifecycle. For details, see [Supported Kubernetes distributions](/docs/latest/operate/kubernetes/reference/supported_k8s_distributions/).

## Bundled Redis database versions

Each cluster version of Redis Software includes a set of Redis database versions as follows:

Redis Software
cluster version
Bundled Redis
DB versions
Default DB version
(upgraded/new databases)

8.2.0
6.2, 7.2, 7.4, 8.0, 8.2, 8.4, 8.6
8.6

8.0.20
6.2, 7.2, 7.4, 8.0, 8.2, 8.4, 8.6
8.6

8.0.18
6.2, 7.2, 7.4, 8.0, 8.2, 8.4, 8.6
8.6

8.0.16
6.2, 7.2, 7.4, 8.0, 8.2, 8.4
8.4

8.0.10
6.2, 7.2, 7.4, 8.0, 8.2, 8.4
8.4

8.0.6
6.2, 7.2, 7.4, 8.0, 8.2
8.2

8.0.2
6.2, 7.2, 7.4, 8.0, 8.2
8.2

7.22.x
6.2, 7.2, 7.4
7.4

7.8.x
6.2, 7.2, 7.4
7.4

7.4.x
6.0, 6.2, 7.2
7.2

7.2.4
6.0, 6.2, 7.2
7.2

6.4.2
6.0, 6.2
6.2

6.2.x
6.0, 6.2
6.0

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page