Alternate Data Streams (ADS) is an NTFS (New Technology File System) file attribute and was designed to provide compatibility with the MacOS HFS (Hierarchical File System). 

● Any file created on an NTFS formatted drive will have two different forks/streams:
○ Data stream -Default stream that contains the data of the file.
○ Resource stream - Typically contains the metadata of the file. 

● Attackers can use ADS to hide malicious code or executables in legitimate files in order to evade detection. 

● This can be done by storing the malicious code or executables in the file attribute resource stream (metadata) of a legitimate file. 

● This technique is usually used to evade basic signature based AVs and static scanning tools

___

## How it works (short)

- NTFS stores files as objects with attributes. The main content is one attribute (the default stream, unnamed), but NTFS allows additional named attributes (the ADS).
    
- Syntax to reference a stream: `filename:streamname` — e.g. `report.txt:notes`.
    
- Only NTFS supports ADS. If you copy the file to FAT, exFAT, some network shares or archive formats that don’t preserve ADS, the alternate streams are typically lost.
    

---

## Legitimate uses

- Metadata and small per-file data without changing the main file (historical: resource forks, metadata).
    
- Windows uses ADS for real features: `Zone.Identifier` (the “Mark of the Web” that Internet Explorer/Edge and SmartScreen use), some thumbnail/cache or backup tools.
    
- Enterprise apps sometimes use ADS for indexing/search metadata or internal bookkeeping.
    

Example: when you download `file.exe` from the web, Windows often writes `file.exe:Zone.Identifier` with contents like: