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

    - Looking at an image and seeing if it has multiple data streams

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

5. We can see this image only has _One_ data stream `Stream        : :$DATA`


## Creating a new Data Stream

1. Open Command Prompt

2. Creating an empty file called ads.txt

    `type nul > ads.txt`

3. check to see if file was created 

    `dir`

4. Lets add a second data stream

    `echo "ADS test" > ads.txt:secret`

5. You can see that the file size is still 0 

    `dir ads.txt`

    `
    05/01/2020 11:18 AM              0 ads.txt
               1 File(s)              0 bytes
               0 Dir(s)  382,149,332,992 bytes free
    `

6. Go into Powershell

    `Get-Item ads.txt -stream *`

    ```Powershell
        PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\ads.txt::$DATA
        PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory
        PSChildName   : ads.txt::$DATA
        PSDrive       : C
        PSProvider    : Microsoft.PowerShell.Core\FileSystem
        PSIsContainer : False
        FileName      : C:\Users\Cory\ads.txt
        Stream        : :$DATA
        Length        : 0

        PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\ads.txt:secret
        PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory
        PSChildName   : ads.txt:secret
        PSDrive       : C
        PSProvider    : Microsoft.PowerShell.Core\FileSystem
        PSIsContainer : False
        FileName      : C:\Users\Cory\ads.txt
        Stream        : secret
        Length        : 13
    ```

    - You can see our secret Stream now

7. Lets check out the contents of that data stream

    `Get-Content ads.txt -stream secret`

    output:
    `"ADS test"`

### We can also add Executables in the data streams and not just text

    - We are going to add an exe to that very same file

1. In CMD run

    - This attachs Notepad.exe into the data stream note

    `type C:\Windows\System32\notepad.exe > ads.txt:note`

2. Back into Powershell

    `Get-Item ads.txt -stream *`

```Powershell
    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\ads.txt::$DATA
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory
    PSChildName   : ads.txt::$DATA
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\Cory\ads.txt
    Stream        : :$DATA
    Length        : 0

    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\ads.txt:note
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory
    PSChildName   : ads.txt:note
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\Cory\ads.txt
    Stream        : note
    Length        : 181248

    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory\ads.txt:secret
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\Cory
    PSChildName   : ads.txt:secret
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\Cory\ads.txt
    Stream        : secret
Length        : 13
```

    - You can see our new note stream, which could have contained a malicious exe