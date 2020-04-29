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

    - create a partition move a Jpeg file into it and then delete it.


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
