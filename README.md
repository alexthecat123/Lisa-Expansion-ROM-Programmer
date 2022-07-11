# Lisa Expansion Card ROM Programmer
A program that lets you modify the contents of Lisa expansion card ROMs without removing them from the Lisa.<br><br>


![IMG_6011](https://user-images.githubusercontent.com/16897189/178359182-c87f20fa-6c8e-4a17-b3c4-637a272cd0c7.png)



# Introduction

I've recently written another program that's really similar to this one that allows you to load code into the Lisa's RAM over serial so that you don't have to type new code in service mode or make a floppy disk to load new code. If that sounds interesting, you can find it [here](https://github.com/alexthecat123/Lisa-RAM-Loader).

After getting my [custom Lisa SCSI card](https://github.com/alexthecat123/Lisa-GALSCSI-Card/) working, I wanted an easy way to program the F-RAM chip that replaces the EPROM in a standard expansion card without having to use an external programmer. This simple assembly language program is what I came up with; it allows you to send a 4K ROM image to the Lisa over serial and writes the image straight to the card's F-RAM chip.

Of course, this program will only work if your expansion card has a writable form of memory (like the F-RAM on my SCSI card) installed; you can't overwrite the contents of something like an EPROM with it.

# Using the Serial Programmer
To use the F-RAM programmer, just write the ExpansionROMProgrammer.dc42 file to a 400K floppy or copy it over to your Floppy Emu. When you boot your Lisa from this floppy, it will scan the slots to see if any cards are installed. If so, it will try writing a value to the first memory location in the card's ROM and reading it back to see if it's writable (don't worry, it resets whatever was in that location back to its original value in case the transfer is aborted). You'll then get a message on the screen telling you which slot (if any) a writable expansion card was detected in and it will ask you to start the serial transfer. Connect a serial cable between your modern computer and the Lisa's serial port B and set your modern computer's terminal software to 57600 baud. Then send your 4K ROM file over as raw binary (no XMODEM or anything fancy like that) and the Lisa should jump back to the monitor once the file has been received and written to the F-RAM. Note that the file must be no less than 4K in size; if it's smaller, the Lisa will just sit there and wait until it receives 4K of data. It's perfectly fine if the file is larger than 4K, but anything after that point will just be ignored.

When my program is scanning expansion slots for valid cards, it starts with slot one and works its way up. It's also incapable of detecting multiple writable cards; once it finds one, it stops scanning and uses that one. So if you have multiple writable cards installed, only the one in the lowest slot number will be found and written to.

Note that the Lisa doesn't eject the floppy after reading the loader program from it; I made this decision because it's a pain to have to reinsert the floppy each time you want to change the contents of the F-RAM if you're making changes really often. If you don't like this feature, it should be pretty easy to modify the source code to eject the disk after booting from it.


# Modifications
All of the code can be found in ExpansionROMProgrammer.x68 and was written for the EASy68K assembler, but it can probably be adapted to other assemblers pretty easily. Keep in mind that I'm still a novice when it comes to assembly language, so it's probably kind of sloppy in places, but it certainly gets the job done!
