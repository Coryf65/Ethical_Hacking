# Alternate Data Streams (ADS)

    - NTFS feature to store extra streams of data beyond the default file data

    - The original purpose of ADS:
        to mirror a similar feature in (Hierarchical  File System)HFS on MacOS

## NTFS File

    - Collection of attribues: file name, security settings, and data

    - Each file attribute has a unique ID - an attribute type code

    - Master file table (MFT): file table used to store the attributes in NTFS

    - MFT conatins both metadata and user data

    - allows multiple data attributes,
        Criminals are able to hide data in their own custom data streams

## ADS Tools

    - ADS-aware


## Demo

1. open Powershell on Windows 10

2. navigate to your pictures folder

    ```PowerShell
        cd .\Pictures\
    ```

3. list out pictures

`dir`

4. pick a file and run the following command

`Get-Item .\YOURIMAGE.jpeg -stream *`


```Powershell
    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\Pictures\memes\51411318_373073860197387_41311267652
                    922038_n-5102184102.jpg::$DATA
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\Pictures\memes
    PSChildName   : 51411318_373073860197387_41311267652922038_n-5102184102.jpg::$DATA
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\Cory\Pictures\memes\51411318_373073860197387_41311267652922038_n-5102184102.jpg
    Stream        : :$DATA
    Length        : 81425

    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\Pictures\memes\51411318_373073860197387_41311267652
                    922038_n-5102184102.jpg:Zone.Identifier
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\Pictures\memes
    PSChildName   : 51411318_373073860197387_41311267652922038_n-5102184102.jpg:Zone.Identifier
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\Cory\Pictures\memes\51411318_373073860197387_41311267652922038_n-5102184102.jpg
    Stream        : Zone.Identifier
    Length        : 182
```

5. We can see this image only has one data stream `Stream        : :$DATA`


## Creating a new Data Stream

1. 