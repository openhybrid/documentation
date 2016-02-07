# Restoring memory card to original state

Once you have installed the hybrid object image to the memory card , the size of the memory card will change (reduce),
normal formatting from windows does not seem to work, here is how you can restore it to original state
you will need Linux OS or Android device

This is the easiest method if formatting from windows didn't work

Android :

1. remove the memory card in your phone if any
2. insert the yun installed memory card into the phone
3. go to storage option in settings
4. under SD card ,select Erase SD card
5. after this your memory card will be back to original capacity

Linux OS :

1. open terminal and type "gparted" , the app will open
2. select you memory card from the devices list
3. unmount all partitions of the SD card
4. delete all partitions
5. add new partition
6. apply all operations
7. your memory card is now back to original state
