## High-Level Overview

```mermaid
flowchart TD
    %% ---------------------------------------------------------
    %% STYLING
    %% ---------------------------------------------------------
    style Canada_Region fill:#ffe6e6,stroke:#ff0000,stroke-width:2px
    style USA_Region fill:#e6f2ff,stroke:#0066cc,stroke-width:2px
    style Europe_Region fill:#e6ffe6,stroke:#009900,stroke-width:2px
    
    %% Highlight Primary Storage
    style R2 fill:#fffde7,stroke:#fbc02d,stroke-width:4px

    %% ---------------------------------------------------------
    %% REGION: CANADA
    %% ---------------------------------------------------------
    subgraph Canada_Region ["🇨🇦 Physical Location: Canada"]
        direction TB
        NodeTO["Smart Node
Location: Toronto, CA
Specs: 50Gbps / 50Gbps, 950GB Flash Storage
Jurisdiction: Canada"]
        
        IA_Van["Internet Archive Mirror
Location: Vancouver, CA
Protocol: HTTP
Jurisdiction: USA"]
    end

    %% ---------------------------------------------------------
    %% REGION: USA
    %% ---------------------------------------------------------
    subgraph USA_Region ["🇺🇸 Physical Location: USA"]
        direction TB
        SourceCoop["Source Cooperative
Location: Oregon, USA
Protocol: HTTP
Jurisdiction: USA"]
        
        R2["☁️ Cloudflare R2
(Primary Object Storage)
Location: Eastern North America
Protocol: HTTP
Jurisdiction: USA"]
        
        IA_SF["The Internet Archive
Location: San Francisco, USA
Protocol: HTTP
Jurisdiction: USA"]
        
        Netcup["Netcup VPS
Location: Manassas, Virginia, USA
Specs: 2.5Gbps / 2.5Gbps, Flash Storage
Protocol: HTTP & BitTorrent
Jurisdiction: Germany"]
    end

    %% ---------------------------------------------------------
    %% REGION: EUROPE
    %% ---------------------------------------------------------
    subgraph Europe_Region ["🇪🇺 Physical Location: Europe"]
        direction TB
        subgraph Netherlands ["🇳🇱 Netherlands"]
            NodeAMS["Smart Node
Location: Amsterdam, NL
Specs: 50Gbps / 50Gbps, 950GB Flash Storage
Jurisdiction: Netherlands"]
        end
        
        subgraph Switzerland ["🇨🇭 Switzerland"]
            Zenodo["Zenodo
Location: Geneva
(Replicated in Budapest)
Protocol: HTTP
Jurisdiction: Switzerland"]
        end
    end

    %% ---------------------------------------------------------
    %% CONNECTIONS
    %% ---------------------------------------------------------
    
    NodeTO <==>|BitTorrent Sync P2P| NodeAMS
    IA_SF -.->|Internal Replication| IA_Van
    
    NodeTO -.->|HTTP Pull| SourceCoop
    NodeTO ==> R2
    NodeTO -.->|HTTP Pull| Zenodo
    NodeTO -.->|HTTP Pull| IA_SF
    NodeTO -.->|HTTP or P2P| Netcup

```

## Infrastructure & Operating Costs

| Service | Description | CAD | USD | EUR |
| :--- | :--- | :--- | :--- | :--- |
| **CDN** | [CDN - Cloudflare Details](https://www.cloudflare.com/plans/) - WAF, CDN (Amortized Annual) | $30.90 | $22.60 | €19.13 |
| **CDN Services** | [Object Storage - Cloudflare Details](https://www.cloudflare.com/products/r2/) & [Serverless - Cloudflare Details](https://www.cloudflare.com/en-ca/plans/developer-platform/) (Variable) | $32.71 | $23.93 | €20.26 |
| **Smart Node 01** | [Decentralized Distribution - SlashN Services Details](https://ultra.cc/#plan-pricing) - Dedicated BitTorrent client | $28.98 | $21.21 | €17.95 |
| **Smart Node 02** | [Decentralized Distribution - SlashN Services Details](https://ultra.cc/#plan-pricing) - Dedicated BitTorrent client | $28.98 | $21.21 | €17.95 |
| **VPS** | [Geospatial Services - Netcup Details](https://www.netcup.com/en/server/root-server) - ARM64 | $14.64 | $10.72 | €9.07 |
| **TOTAL** | **Monthly Run Rate** | **$136.21** | **$99.67** | **€84.36** |

**Note:** Currency conversions are based on rates from February 16, 2026.

## Roadmap: Resilience & Transparency

To support our mission of providing high-performance, analysis-ready data, we are currently developing a suite of public tools to make this distributed ecosystem more **FAIR** (Findable, Accessible, Interoperable, Reusable), **resilient**, and **transparent**.

These planned features are designed to help researchers and automated systems coordinate data access across the various platforms and mirrors we utilize.

### 1. Real-Time Service Status

We are building a comprehensive status dashboard that monitors the availability of the diverse storage locations we rely on, from our own Smart Nodes to external providers like the Internet Archive, Source Cooperative, and Zenodo. Users will be able to verify if a specific mirror is operational before initiating workflows.

### 2. Traffic & Load Optimization Statistics

To foster better cooperation between our systems and downstream users, we plan to expose traffic and connectivity statistics where possible.

This transparency allows automated systems to be "smart" about data retrieval. For example, a system could query these statistics to schedule bandwidth-intensive HTTP downloads during non-peak hours, or adjust behavior based on current connectivity loads during high-traffic periods of the workday. This improves performance for individual users while respecting the bandwidth constraints of the various host providers.

### 3. Community Issue Reporting

We are introducing a streamlined method for users to report access issues across any of the services we aggregate. By allowing the community to flag connectivity drops or data integrity issues quickly, we can identify bottlenecks or outages at specific providers and route users to alternative sources more effectively.
