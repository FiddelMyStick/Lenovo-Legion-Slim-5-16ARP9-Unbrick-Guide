# Lenovo-Legion-Slim-5-16ARP9-Unbrick-Guide
This guide details the successful recovery of a [Legion Slim 5 16ARP9] after Lenovo Vintage update bricked it. The method involves hardware flashing using a CH341A programmer and a DIY pressure method for the WSON-8 chip solder-less! (USE IT YOUR OWN RISK IF YOU ARE NOT AFRAID TO LOSE WARRANTY OR IN CASE YOU ALREADY HAVE IT)

## What You'll Need
*   CH341A Programmer
*   **1.8V Adapter** (CRITICAL: The chip runs on 1.8V, not 3.3V or 5V) (For my chip it was [W25R256JWEIQ TR], Identify and Read yours so you evade any dammages to your chip !)
*   WSON-8 Probe Solder-Less (8x6mm)
*   **Tools for DIY Pressure**: An adjustable wrench, a stack of books, a friend for initial alignment.
*   Software: `NeoProgrammer`, `HxD`, `H2OEZE` (for diff analysis, and replacing modules [Insyde tools]), `Insyde iFlash Image Extractor`.

## Step-by-Step Flash Process

### 1. Preparation & Dumping
*   *Classic open and bust out your laptop and locate the chip, It will be obvious if it says "Winbond", then you've hit the jackpot read the serial and search it *
*   *Make sure the bios chip is exposed and ready to proceed for the next steps ALSO REMOVE ANY POWER SOURCES YOU HAVE NOTHING SHOULD BE PLUGED TO THE MOTHERBOARD ! [CMOS, BATTERY, AC]*

### 2. The "Wrench-and-Books" Pressure Technique
Holding the probe steady is the hardest part. The `SOIC8` clips don't fit the WSON-8 pads. Instead:
1.  Carefully align the WSON-8 probe over the chip.
2.  Place the head of an **adjustable iron wrench** in way that holds the wson-8 like a hand.
3.  Have an assistant hold the wrench in perfect alignment.
4.  **Gently** stack books on the wrench's handle to weigh it down and apply even pressure. (Avoid excessive force).
5.  *![Project Logo](/assets/a58e5707-b605-41a6-b100-ca62a3e5b3c5.jfif)*
6.  *![Project Logo](/assets/122becf8-47f1-40d5-8223-c7987abf5760.jfif)*
7.  *![Project Logo](/assets/b469fb91-5402-4355-b8e4-888764055cfd.jfif)*
8.  *![Project Logo](/assets/14728db3-aae2-48e4-b1fa-a73fe435247b.jfif)*

### 3. Extracting the Boot-Corrupted Dump
*   *Now into the hardest part*
*   *Use your CH341 aka programmer alongside your 1.8v adapter and the wson-8 probe that you've alinghed earlier and puted it down with neoprogrammer to detect the chip and read it*
*   *Beware the you will need CH341DLL (driver for your programmer) for the software itself can detect your "CH341"*
*   *Now once all good try to detect the chip and move the wson-8 probe accordinglly until neoprogrammer detects it, and then HOLD STILL, APPLY WEIGHT TO THE WRENCH to hold the position for you and make ur life ez*
*   *Once all there, and the chip is detected, read the BIOS more than 2 times at least until you get the same reading for at least 2/3 times to you actually know that u extracted the right BIOS code*
*   *Then we can move to the next part which is the analysis/Getting the right working BIOS*

### 4. Analyzing & Isolating the Damage
Using `H2OEZE` and `HxD`: (I Will include softwares I used here for you to free to exploit as you want)
1.  `Insyde iFlash Image Extractor` to split the Lenovo update file.
2.  `H2OEZE` to compare the dump with a clean BIOS to pinpoint the corrupted area.
*   *Welcome to the most challenging part, now what you need to do now is to go to the official website of your laptop using S/N or Name (for my case was Legion slim5 16arp9), drivers and BIOS, then install
*   your desired BIOS Version that was already compatible with the bricked one (Here you can use H2OEZE to inspect the old bios like this)*
*   *![Project Logo](/assets/HOEZ.PNG)*
*   *Beware that this is only applicable for insyde type of firmwares*
*   *If you dont know which version, then you MUST INSTALL all available BIOS version that are there and compare them manually with HxD (Diff) after extraction [Step 2]*
*   *Now after you have the manufacturer's BIOS, now you have to extract it, you can use 7zip to reveal the (fd, ROM, bin) or the BIOS file (actual one) that will be the biggest among the extracted .exe file*
*   *If that dosent work, use innoextract.exe (Follow instructions here "https://github.com/dscharrer/innoextract") instead to reveal it*
*   *Once you have the file for my case it was .ROM that was bigger than my actual BIOS so, next I had to extract the BIOS Region from it (my Original BIOS was 32MB) so we need to match it*
*   *Here for insyde tools, I used "Insyde iFlash Image Extractor", just take the .ROM/ .fd/ .bin and drop it on the extractor.exe for it to give you the BIOS you need as it will create a folder that contains it*
*   *![Project Logo](/assets/extractor.PNG)*
*   *![Project Logo](/assets/extractor_res.PNG)*
3.  `HxD` for a manual offset inspection.
*    *Now the confusing part, which is the diffs and finding your DMI data*
*    *It's quite overwhelming at first but once you get the hang of it it's just copy paste and you don't need to understand binary to do it lol*
*    *Here wasmy exemple in each, H2OEZE (If you dont have this or not working for you it's fine, you can use HxD for diffing)*
*    *To know that we indeed extracted the BIOS region, you will find that both broken and the extracted bin have the same header (BIOS region identifier)*
*    *And if you do a difference, you may not see a very BIG diffs, since it's the same version as the old bricked, and only toched corrupted parts are the highlighted differences*
*    *If in your case you don't have a backup for the original then you need to seek someone has the same model as you and give you backup ! as you might need it to compare, otherwise u will just be drowning and praying that some BIOS that you will extract magically works !*
*    *![Project Logo](/assets/Same.PNG)*
*    *![Project Logo](/assets/HODiff.PNG)*
*    *If you managed to reach here then you are pretty much almost done, the rest is gonna be finding your DMI data*
*    *So hop on HxD and type your Model name like this*
*    *![Project Logo](/assets/ARP.PNG)*
*    *And then highlight your whole DMI until the padding where it stops with FFFFFFF copy it and then paste it in the same offset in you extracted BIOS like this: *
*    *![Project Logo](/assets/ARP_Paste.PNG)*
*    *This is IMPORTANT please always make copies so you dont temper with original backups and extractions and avoid any repeated work and good luck*
*    *If you are curious to know what on your BIOS and diff more with understandable text, insall and use UEFI_Tool, and see the differences yourself with the features it gives (aka drivers etc...)*
*    *Once this phase is finished we can then go and try to reflash our DMI fixed Extracted BIOS*


### 5. Flashing the Recovery Image
*   *Now It's just a matter of writing the extracted BIOS (with DMI) into your chip*
*   *Using the setup from before, go to neoprogrammer and erase the chip (MAKE SURE YOU HAVE ALWAYS THE BACKUP OF YOUR ORIGINAL BIOS), wait and then after 1min write the extracted_bios(that has your DMI) into the chip, and wait for it the process to finish*
*   *Afterwards make sure that you've written the correct bin file by reading again and checking the CRC (Hash) that indeed we did write the correct file*
*   *Remove the Flasher (wson-8 probe) and hook back the CMOS and the AC, then turn it on and wait for light to be back again, you might need to give the pc some time bfr it turns on since it has to adapt with the new BIOS and make it compatible again with the HARDWARE*
*   *Warning you might see an EC differnece in case you flashed a diff BIOS version but that won't hurt, you just need to boot into windows again and then update the bios via the .exe file for the EC to match your BIOS Version, for my case the EC binary comes within one whole file (which is the one I extracted earlier)*
  
### From my perspective I tried a version with NO DMI and still worked, so you can skip the DMI part if you feel overwhelmed and start flashing and pray it turns on, otherwise you have to copy ur DMI as for some types of motherboards is a requirement.

## Critical Warnings
*   **1.8V Adapter**: Do not skip this.
*   **Patience**: The probe can take many attempts. The DIY pressure method may need adjustments.
*   **First Boot**: After flashing, the first boot might take [5 minutes]. Don't panic.

## Acknowledgements
*   Shout-out to the open-source community for providing the tools, and knowledge that made this possible.

*This guide is provided for informational purposes. Proceed at your own risk.*
