# Pcileech-SimpleFirmwareCustomizationGuide

**🤬I hate scammers who make crap firmware and sell it at inflated prices, and people who like to be vague in their guidelines.**

**😊That's why this guide was created, to make your own proprietary firmware with zero experience!**

### Emphasis
* This guide teaches you to make your own firmware using open source projects instead of starting from scratch!

* The guide follow step by step, enough to make firmware for BE!

* This guide uses descriptions that even primary school children can understand!

### How to get help

discord：https://discord.gg/beater

🔥Beaters DC channel offers free firmware (Many scammers firmware distributors are angry because Beater has made VMD firmware available for free，Try to discredit Beater by claiming that the free firmware is unsafe so that users can buy their paid Poor quality firmware.),BeaterFreeVMD play the games you want to play!VGK has implemented VMD detection not long ago, and EAC BE still runs well. Beater Never tried to sell VMD firmware solutions for profit.

# Let's start making firmware！

### Capturing real configuration space

Beginners naturally take the easiest network card solid as a demonstration (demonstration of the use of ekknod open source WIFI source code, let us thank ekknod), to buy a PCIe network card, the price does not matter, inserted into a computer that can run normally, pay attention to the PCIe network card, do not buy the wrong other interfaces! Acquisition software recommended TeleScan more convenient, but TeleScan default display bar value is wrong, you need to manually repair (the following will be demonstrated), skilled hands can use Arbor. production process is not only applicable to the network card, you have other firmware source code can also be done.

Capture the real configuration space open TeleScan to find the network card you bought (class more bad to find you can choose other arrangement), see the picture box out of the card's Base Address Register column that Bar? Left-click to select Bar0 after the box will become blue, in the right side of the long bar in the digital box, long press 1 to replace all the numbers in the box for 1, at this time, the blue selected box value and the long box number 1 are green, right-click to select the Write ¡ú DOWORD to save (you can use Alt + Shift + D shortcut keys to save), save the changes in the blue selected box value and the long bar box value will become black! After saving the changes, the values in the blue checked box and the values in the long bar box will turn black, and so on, Bar0~Bar5 will be repaired to the correct Bar value (only Bar0/Bar2/Bar4 in the figure have values to be repaired). After fixing the Bar values, you can save the tlscan file (the save button is the second one in the second row).

![image](https://github.com/user-attachments/assets/921485fb-7e5d-4231-9652-e082a11bf311)
![image](https://github.com/user-attachments/assets/5ea4bdb4-a1b4-450d-95fd-8a35e43b60b3)


### pcileech_cfgspace.coe

Nowadays there are many one-click tlscan to COE tool, Just find a tlscan to COE tool to convert , the generated file renamed pcileech_cfgspace.coe replaced into the IP folder you use the source code, overwrite the original pcileech_cfgspace.coe file.

![image](https://github.com/user-attachments/assets/57e92ce6-b12a-4434-9e02-25d244c87d2b)

### pcileech_fifo.sv pcileech_pcie_cfg_a7.sv

pcileech_fifo.sv file in your source code decompression src folder, you can use Vivado/Notepad++/Notepad to open the modification (depending on personal habits, the final firmware are to use Vivado to generate, Vivado open the path to srcs/sources 1/imports/pcileech- xxx/src/pcileech-fifo.sv), open the pcileech_fifo.sv file see the picture boxed out of the rw[203] and rw[206] two places? rw[203] of the b after the change of 0, rw[206] after the change of b after the change of 1. change remember to save!

![image](https://github.com/user-attachments/assets/d0804606-12e5-4803-953b-6ebf04d53dd4)

If you want to change the value after rw[143:128]~rw[199:192], you can fill in the corresponding value read by TeleScan backwards or randomly (the effect is the same).

![image](https://github.com/user-attachments/assets/2748df3c-6ca2-4815-ab28-a3120cdc849a)

![image](https://github.com/user-attachments/assets/95a5ef89-d9bc-4126-8ef7-83d7f1906d0a)



After changing the pcileech_fifo.sv file now change the pcileech_pcie_cfg_a7.sv file, also in the src folder after you unzipped the source code, what software to use to change the same depends on personal habits (Vivado open the path to srcs/sources 1/imports/pcileech-xxx/ src/pcileech fifo.sv). src/pcileech fifo.sv), see the rw[127:64] boxed out in the picture, the 16-bit value after h is the firmware DSN, here fill in the DSN (abbreviation of Device Serial Number Capability) read by TeleScan to your card, the 1st DW+2nd DW value added together is not exactly 16 bits? The value in 1st DW+2nd DW should be 16 bits, the value in 2nd DW should be filled in the front, the value in 1st DW should be filled in the back, don't fill in the opposite. Then change rw[21] to 0 and rw[20] to 1 (here it is recommended to keep the default unchanged). change remember to save!

![image](https://github.com/user-attachments/assets/0014d44b-59fc-47ee-88e7-36a1f51740ff)
![image](https://github.com/user-attachments/assets/8378ad7e-88c4-417e-b8ef-eea7b726c76d)

If you want to change the value after rw[143:128] and rw[199:192], you can fill in the corresponding value read by TeleScan upside down or just fill in the corresponding value (the same as the above rw[143:128]~rw[199:192]-fill in the random value needs to be consistent).

![image](https://github.com/user-attachments/assets/7fb56ebb-f260-43b8-a033-a79c8e542643)


### pcie_7x_0_core_top.v

This document is recommended to use other people to change the good version of the province of a pointer to check, after all, the public source code and then how to sew patch is not too bad, is not it? The pursuit of hand-operated words in accordance with the following chart in the order of control can be modified, the first ID board is very simple to modify the box h after the value changed to TeleScan to read out the corresponding parameters of the card can be, the other do not make any changes.

![image](https://github.com/user-attachments/assets/979c7a2c-c5ff-41ea-9ed7-04b27287c2db)

The second BAES board fills in the offset offset address of the NIC read out by Telesan, i.e., the value before h after offset in the Advanced Error Reporting Capability and Virtual Channel Capability brackets, which only needs to be changed in two places.

![image](https://github.com/user-attachments/assets/7298fccb-4af9-4488-9071-735dc53aebbb)

The third BAR board fill Telesan read out the value of the network card's Base Address Register0~5, 04 and 04 end of the BAR after the BAR full F, for example, BAR1/BAR3/BAR5. because bar0 can not be 1 at the end of the 1 is on behalf of the 10, 0 is on behalf of the memory space to be changed to 0 bar2 and bar4 followed by the 4 or C is the 64-bit address, then bar3 and bar5 should be changed to all F.

![image](https://github.com/user-attachments/assets/97e46cd2-644f-45e4-b75f-77d1fbf784ec)

The following one by one to check and find the changes, as shown in the figure against the modification of each place, Telesan read out the network card has to change to the corresponding value and open (FALSE to TRUE), there is no skip.

![image](https://github.com/user-attachments/assets/64848fbb-b6e6-41b0-a7db-5601ef32f090)
![image](https://github.com/user-attachments/assets/8008baf6-9a89-4d8f-a210-ca01d7fae33c)
![image](https://github.com/user-attachments/assets/824970e5-ea8c-4790-989b-022ce77c2179)
![image](https://github.com/user-attachments/assets/88bb4176-d6b6-442b-b849-af70bf2c0697)
![image](https://github.com/user-attachments/assets/55a4edc1-41e3-42b0-b71b-cdc98ecf0f74)
![image](https://github.com/user-attachments/assets/4cb13634-f0be-48ad-bb74-9030d3437b9a)
![image](https://github.com/user-attachments/assets/471f914b-fb47-4cba-a954-5b1ab9135875)
![image](https://github.com/user-attachments/assets/b6b15064-a9ab-4a1e-984f-27bee2ffaf56)
![image](https://github.com/user-attachments/assets/09a4c272-e195-4f5d-86de-9346c9363187)
![image](https://github.com/user-attachments/assets/fef12c92-f1d0-4688-86de-e858e7b93203)
![image](https://github.com/user-attachments/assets/88a900b2-629c-4b96-aaca-e169b73fa046)
![image](https://github.com/user-attachments/assets/1a12dcc2-4c9c-496b-a94c-3efe74ead864)
![image](https://github.com/user-attachments/assets/e94e57d9-a884-461c-b5c9-9a86502dfa4f)
![image](https://github.com/user-attachments/assets/f9fea5cc-332b-4b0c-8fb6-9bd4d0feb770)
![image](https://github.com/user-attachments/assets/897d8ace-9abb-47de-bb71-47eac1cd7158)
![image](https://github.com/user-attachments/assets/2c2d476d-4108-42b0-b4c0-3c951b812321)
![image](https://github.com/user-attachments/assets/db0fdaa4-929e-40ce-886a-e27939ad5e77)

### Let's generate the firmware.

Vivado left navigation bar click Generate Bitstream, pop-up window box out of the option to generate the firmware when the number of CPU cores used, the recommended default core number, if your computer configuration is higher can be appropriate to increase the output core. Generate the firmware pcileech _enigma_x1 _top.bin in the source code file under the impl_1 folder.

![image](https://github.com/user-attachments/assets/af09824b-143f-4639-9a0b-344104ce69f0)

# Making a personal firmware based on open source source modifications is easier than you think, isn't it?

😉Want to add more advanced features to the firmware to deal with stronger anti-cheats? I'll keep compiling guides when I have time!







