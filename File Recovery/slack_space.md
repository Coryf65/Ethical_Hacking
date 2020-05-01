# Slack Space

    -

1. using fsutil we can see info about out filesystem

    `fsutil fsinfo ntfsinfo c:`

```CMD
    NTFS Volume Serial Number :        0xf2bebd4abebd07dd
    NTFS Version      :                3.1
    LFS Version       :                2.0
    Total Sectors     :                974,559,073  (464.7 GB)
    Total Clusters    :                121,819,884  (464.7 GB)
    Free Clusters     :                 93,367,826  (356.2 GB)
    Total Reserved Clusters :                6,260  ( 24.5 MB)
    Reserved For Storage Reserve :               0  (  0.0 KB)
    Bytes Per Sector  :                512
    Bytes Per Physical Sector :        512
    Bytes Per Cluster :                4096
    Bytes Per FileRecord Segment    :  1024
    Clusters Per FileRecord Segment :  0
    Mft Valid Data Length :            823.00 MB
    Mft Start Lcn  :                   0x00000000000c0000
    Mft2 Start Lcn :                   0x0000000000000002
    Mft Zone Start :                   0x0000000000000000
    Mft Zone End   :                   0x0000000000000000
    MFT Zone Size  :                   0.00 KB
    Max Device Trim Extent Count :     512
    Max Device Trim Byte Count :       0xffffffff
    Max Volume Trim Extent Count :     62
    Max Volume Trim Byte Count :       0x40000000
    Resource Manager Identifier :      E4E3156B-B1F6-11E8-9FB2-8ED2F772D6EB
```

    - I have 512 Bytes per sector, and 4096 Bytes per cluster

![sector image](/_images/slackSpace.PNG)