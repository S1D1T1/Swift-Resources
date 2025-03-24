graph TD
    CheckLoc([Source Location])
    
    CheckLoc --> ExifUserComment[EXIF UserComment]
    CheckLoc --> TiffImageDesc[TIFF ImageDescription]
    CheckLoc --> PngChunk[PNG Custom Chunk]
    CheckLoc --> PngMeta[PNG Metadata]
    CheckLoc --> NoDataLoc[No Data Found]
    
    ExifUserComment --> IsJson([Is JSON?])
    IsJson -->|Yes| CheckDTKeys([Has 'v2' key OR both 'c' and 'uc' keys?])
    CheckDTKeys -->|Yes| DrawThings[Draw Things]
    CheckDTKeys -->|No| CheckCivKeys([Has keys with 'civitai' OR 'cfg scale' OR 'ksampler' OR has 'resources' array?])
    CheckCivKeys -->|Yes| Civitai[Civitai]
    CheckCivKeys -->|No| Generic[Generic JSON]
    
    IsJson -->|No| UnstructuredText([Unstructured Text])
    UnstructuredText -->|Contains 'cfg scale'| CivitaiText[Civitai]
    UnstructuredText -->|Otherwise| DTText["Draw Things (legacy)"]
    
    TiffImageDesc --> TiffText([Unstructured Text])
    TiffText -->|Contains '--'| MidJourney1[MidJourney]
    
    PngChunk --> ChunkName([Check Chunk Name])
    ChunkName -->|Contains 'invoke'| InvokeAI[InvokeAI]
    ChunkName -->|Is 'comf'| ComfyUI1[ComfyUI]
    ChunkName -->|Is 'parameters'| CheckVersion([Check Version])
    CheckVersion -->|Contains 'fooocus'| Fooocus[Fooocus]
    CheckVersion -->|Otherwise| A1111_1[A1111]
    
    ChunkName -->|Other| CheckComfyContent([JSON has 'workflow' OR 'prompt' as object?])
    CheckComfyContent -->|Yes| ComfyUI2[ComfyUI]
    CheckComfyContent -->|No| Unknown1[Unknown Format]
    
    PngMeta --> PngMetaCheck([Check PNG Metadata])
    PngMetaCheck -->|'software' contains 'midjourney'| MidJourney2[MidJourney]
    PngMetaCheck -->|'software' contains 'stable diffusion'| A1111_2[A1111]
    PngMetaCheck -->|'rawText' contains '--'| MidJourney3[MidJourney]
    PngMetaCheck -->|Otherwise| Unknown2[Unknown Format]
    
    NoDataLoc --> NoData["¯\\_(ツ)_/¯"]
    
    %% Style for source locations
    style ExifUserComment fill:#ffcc99,stroke:#ff9900,stroke-width:2px
    style TiffImageDesc fill:#ffcc99,stroke:#ff9900,stroke-width:2px
    style PngChunk fill:#ffcc99,stroke:#ff9900,stroke-width:2px
    style PngMeta fill:#ffcc99,stroke:#ff9900,stroke-width:2px
    style NoDataLoc fill:#ffcc99,stroke:#ff9900,stroke-width:2px
    
    %% Style for formats
    style DrawThings fill:#d4f0fd
    style Civitai fill:#d4f0fd
    style MidJourney1 fill:#d4f0fd
    style MidJourney2 fill:#d4f0fd
    style MidJourney3 fill:#d4f0fd
    style ComfyUI1 fill:#d4f0fd
    style ComfyUI2 fill:#d4f0fd
    style InvokeAI fill:#d4f0fd
    style Fooocus fill:#d4f0fd
    style A1111_1 fill:#d4f0fd
    style A1111_2 fill:#d4f0fd
    style NoData fill:#d4f0fd
    style CivitaiText fill:#d4f0fd
    style DTText fill:#d4f0fd
