# Source Location Decision Tree

```mermaid
graph TD;
    A[Source Location] --> B[EXIF "UserComment"]
    A --> C[TIFF "ImageDescription"]
    A --> D[PNG Custom Chunk]
    A --> E[PNG Metadata]
    A --> F[No Data Found]

    B --> G[Contains JSON]
    B --> H[Contains Unstructured Text (later converted to JSON)]
    G -->|Has "v2" key OR both "c" and "uc" keys| I[Draw Things]
    G -->|Has keys containing "civitai" OR "cfg scale" OR "ksampler" OR "resources"| J[Civitai]
    H -->|Text contains "cfg scale"| J
    H -->|Otherwise| I

    C --> K[Contains Unstructured Text]
    K -->|Text contains "--"| L[MidJourney]

    D --> M[All contain JSON]
    M -->|Chunk name contains "invoke"| N[InvokeAI]
    M -->|Chunk name contains "comf" OR JSON contains "workflow" key OR JSON contains "prompt" as object (not string)| O[ComfyUI]
    M --> P[Chunk name is "parameters"]
    P -->|JSON["version"] contains "fooocus"| Q[Fooocus]
    P -->|Otherwise| R[A1111 (Stable Diffusion WebUI)]

    E -->|Software field contains "midjourney"| L
    E -->|Software field contains "stable diffusion"| R
    E -->|RawText contains "--"| L

    F --> S[Image dimensions synthesized as parameters]
