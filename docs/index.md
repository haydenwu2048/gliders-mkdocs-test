# Welcome to Gliders Documentation 🚀

## Choose where to start:

- [How-to Guides](how/ssh-key.md)
- [Glider Data Pipeline](gdp/gdp.md)

## Mermaid Demo
!!! tip
    Use either `pymdownx.superfences` extension or `mermaid2` plugin to display mermaid diagrams in MKDocs. 

**Example**:
```yaml
- pymdownx.superfences:
  custom_fences:
    - name: mermaid
      class: mermaid
      format: !!python/name:pymdownx.superfences.fence_code_format
```

### Data flow
``` mermaid
    %%{
      init: {
        'theme': 'base',
        'themeVariables': {
          'primaryColor': '#A3F7B5',
          'primaryTextColor': '#000',
          'primaryBorderColor': '#000',
          'lineColor': '#444',
          'secondaryColor': '#FAFAFA',
          'tertiaryColor': '#FFD166'
        }
      }
    }%%
    
    flowchart LR
        %% Nodes
        data_acq@{ shape: circle, label: "Data Acquisition"}
        qc((QC))
        schemas@{ shape: docs, label: "Project Schemas (tags, station deployments, detections)"}
        obis[obis]
        discovery[discovery]
        geoserverSchema["geoserver"]
        geoserver(("Geoserver"))
        erddapSchema["erddap"]
        erddap["ERDDAP"]
        web[🌐 Web Publishing]
        db@{ shape: lin-cyl, label: "Database"}
    
        %% Flows
        data_acq --> schemas
        qc --> schemas
        schemas ~~~ 
        %% schemas ~~~ is used for positioning the QC to Project Schemas, without using it, lines will be crossed
        schemas --> qc
        schemas -- moorings, animals, tags --> obis
    
        obis -- publication scripts --> erddapSchema
        obis -- summaries / node aggregation --> discovery
        
        discovery --> web
        discovery -- nodes --> db
        discovery ~~~
        %% similarly ~~~ is used to prevent line crossing
        discovery -- project metadata--> geoserverSchema
    
        obis -- station data --> geoserverSchema
        geoserverSchema --> geoserver
        erddapSchema --> erddap
    
        %% Styles
        classDef schemaNode fill:#EEE6FF,stroke:#000000,stroke-width:1px,color:#000000;
        classDef obisNode fill:#CFCFCF,stroke:#CFCFCF,stroke-width:1px,color:#000000,font-weight:bold;
        classDef discoveryNode fill:#FFD166,stroke:#FFD166,stroke-width:1px,color:#000000;
        classDef erddapNode fill:#2294A8,stroke:#2294A8,stroke-width:1px,color:#fff;
        classDef geoserverNode fill:#fff,stroke:#58A4D4,stroke-width:2px,color:#58A4D4, font-weight:bold;
        classDef webNode fill:#F0EFEB,stroke:#999999,stroke-width:1px,color:#000000;
        classDef dbNode fill:#A0C4FF,stroke:#000,stroke-width:1px,color:#000000;
        classDef commonSchemaNode fill:#FFD166,stroke:#FFD166,stroke-width:1px,color:#000000;
    
        class schemas schemaNode;
        class obis commonSchemaNode;
        class discovery commonSchemaNode;
        class erddapSchema commonSchemaNode;
        class geoserverSchema commonSchemaNode;
        class geoserver geoserverNode;
        class web webNode;
        class db dbNode;
        class erddap erddapNode;
```
### OBIS Schema Integration

```mermaid
    flowchart BT
        %% Tag Flow
        subgraph Tag
            tag_start(( )):::step0_start --> tagging_meta_sheet[[tagging metadata sheets]]:::step1
            tagging_meta_sheet --> tag_raw(c_tag_meta_suffix):::step2
            tag_raw --> animcache(animalcache_suffix):::step3
            tag_raw --> tagcache(tagcache_suffix):::step3
            animcache --> otnanim(otn_animals):::step4
            tagcache --> otntra(otn_transmitters):::step4
        end
    
        %% Instrument Deploymetns Flow
        subgraph Instrument Deployments
            rcv_start(( )):::step0_start --> rcv_sheet[[deployment metadata sheets]]:::step1
            rcv_sheet --> rcv_raw(c_shortform_suffix):::step2
            rcv_raw --> stat(stations):::step3
            rcv_raw --> rcv(rcvr_locations):::step3
            stat --> moor(moorings):::step4
        end
    
        %% Detections FLow
        subgraph Detections
            det_start(( )):::step0_start --> det_sheet[[detection instrument data]]:::step1
            det_sheet --> event_raw(c_events_suffix):::step2
            event_raw --> events(events):::step3
            det_sheet --> det_raw(c_detections_suffix):::step2
            det_raw --> det(detections_yyyy):::step3
            det --> otndet(otn_detections_yyyy):::step4
        end
    
        %% Integration
        subgraph OBIS Schema Integration
            rcv --> moor
            events -.-> moor
            %% OBIS schema integration
            otntra --> obismoor(moorings):::step5
            otnanim --> obisanim(otn_animals):::step5
            moor --> obismoor(moorings):::step5
            otndet --> obisdet(otn_detections_yyyy):::step5
            obisanim --> obis[(Parent schema)]:::step5
            obismoor --> obis
            obisdet --> obis
            obis --> done(( )):::step6_end
        end
    
    
        %% Styles
        classDef step0_start fill:#A3F7B5,stroke:#000,stroke-width:1px;
        classDef step1 fill:#EEE6FF,stroke:#000,stroke-width:1px;
        classDef step2 fill:#A0C4FF,stroke:#000;
        classDef step3 fill:#BDB2FF,stroke:#000;
        classDef step4 fill:#FFD166,stroke:#000;
        classDef step5 fill:#CFCFCF,stroke:#000;
        classDef step6_end fill:#F08080,stroke:#000;
        classDef invisible fill:none,stroke:none;
        style done fill:#F00,stroke:#000;
```

## Admonitions

!!! tip
    Tip block

!!! warning
    Warning block

!!! note
    Note block

!!! info
    Info block

!!! success
    Success block

!!! danger
    Danger block

!!! example
    Example block

??? info "Click to expand Wave Glider notes"
    - note 1
    - note 2

## Mathematics
The depth calculation from pressure:

$$
\begin{aligned}
P &= P_0 + \rho g z \\
z &= \frac{P - P_0}{\rho g}
\end{aligned}
$$
