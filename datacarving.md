# Data Carving Technique

## Background

- Data carving is a technique to locate the start and end of a file, as well as where its 
    fragments are in between to eventually retrieve the original file in its entirety.

1. Data carving is necessary because criminals tamper with files. One of the most common temporary method 
    is to delete a file, which doesn't actually erase this data but simply removes an entry in a file table, such as fat or inode.

2. The second technique is to alter file extensions to mislead investigators.

3.  The third trick is to hide information in a slack space, which is the remaining free space after 
        creating file in a cluster or block. Once again, data carving comes in handy here. 

## Example:

    - Scenario: create a partition move a Jpeg file into it and then delete it.

    - .Jpeg files always start with JFIF


1. Plug an empty USB drive into PC

2. Windows Key + X, type: `diskpart.exe` and run

3. Type `list disk`

    ```cmd
        Disk ###  Status         Size     Free     Dyn  Gpt
        --------  -------------  -------  -------  ---  ---
        Disk 0    Online          465 GB  1024 KB        *
        Disk 1    Online          465 GB      0 B
        Disk 2    No Media           0 B      0 B
        Disk 3    No Media           0 B      0 B
        Disk 4    No Media           0 B      0 B
        Disk 5    No Media           0 B      0 B
        Disk 6    Online          960 MB      0 B
    ```

4. find which device is the USB (mine is Disk 6, 1 Gig Flash Drive)

5. type `select disk 6` replace with your number

    ```cmd
        Disk 6 is now the selected disk.
    ```

6. Lets initialize our USB __WARNING__ This will erase __ALL Data__ on that drive/usb.
    type `clean`

    ```cmd
        DiskPart succeeded in cleaning the disk.
    ```

7. Now create a new parition of 100MB type: `create partition primary size=100`

    ```cmd
        DiskPart succeeded in creating the specified partition.
    ```

8. Second Partition to tkae the rest of the space

    ```cmd
        create parition primary
    ```

9. see the Partitions we created by: `list patition`

```cmd
    Partition ###  Type              Size     Offset
    -------------  ----------------  -------  -------
    Partition 1    Primary            100 MB    64 KB
  * Partition 2    Primary            859 MB   100 MB
```

10. Now select Partition 1

    ```cmd
        select partition 1
    ```

11. Format the Partition

    `format quick fs=fat32 label="carving"`

12. Now select the other partition

    ```cmd
        select partition 2
    ```

13. Format this partition with NTFS

    `format quick fs=ntfs label="data"`

14. type `exit` to quit Diskpart

## Using a Linux Machine plugging USB stick in (to investigate)

1. search for text finding JFIF 

    `grep JFIF carving.dd`
    
    - we found a match

2. Where the string match is

    `grep -oba JFIF carving.dd`

    - Offset value = 
    `4198406:JFIF`

4. Convert Binary to Hex

    `xxd carving.dd | grep 'd9 ff'`

    - you will see lots of matches but let's find closest to the offset value

5. Convert Offset Value: `4198406:JFIF` to Hex for searching

    `echo "obase=16; 4198406" | bc`

    - `401006`

6. Now we will carve the data we want out of our file

    - Location of the trailer (ff d9): 0x4110a0 = 4264096

    - Beginning sector number: 40106/512 = 8200

    - Ending sector number: 4264096/512 = 8328

    - The number of sectors to be extracted: 8328-8200 = 128 sectors

7. Extract the Picture file whose size = 128 sectors

    `dd if=carving.dd of=carved.jpg bs=512 skip=8200 count=128`

8. Got it!
![Recovered!](/_images/Carving_Finished.PNG)