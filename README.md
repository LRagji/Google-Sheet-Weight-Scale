# Google-Sheet-Weight-Scale
The repo contains Arduino code and reverse engineering experience for Body Composition Scale and how to hook up the same onto google sheet
[Check out the Wiki for more details](https://github.com/LRagji/Google-Sheet-Weight-Scale/wiki)

## Decoding UART protocol:
> This was like decoding Matrix :sunglasses:
#### Weight frame decoding
00 <-Series of 8 Sync byte 0  
00 <-Sync byte 1  
00 <-Sync byte 2  
00 <-Sync byte 3  
00 <-Sync byte 4  
00 <-Sync byte 5  
00 <-Sync byte 6  
00 <-End of 8 Sync byte 7  
02 FE 00 00 00 CC CA <- ?  
02 FE 07 00 00 CC D1 <- ?  
02 FE 06 FE 00 CC CE <- ?  
02 00 00 00 00 **CE** CE <- Weighing under progress CE status byte  
02 **01 E2** 00 00 CE B1 <- Weighing under progress 1E2=482=**48.2Kg**  
02 01 E2 00 00 CE B1  
02 01 E2 00 00 CE B1  
02 01 E2 00 00 CE B1  
02 01 E3 00 00 CE B2  
02 01 E3 00 00 CE B2  
02 01 E4 00 00 CE B3   
02 01 E3 00 00 CE B2   
02 01 E4 00 00 CE B3   
02 **01 E2** 00 00 **CA** AD <-Confirmed as Status byte is set CA 1E2=482=**48.2Kg**  
#### Body composition frame decoding
02 FD 00 00 00 CB C8  
02 FD 01 03 B3 CB 7F  
02 FE 0F 00 00 CC D9  
02 FE 00 01 E2 CB AC  
02 FE 01 00 B0 CB 7A  
02 **FE 02 01 1D CB** E9 <- FAT%(02 3rd byte and CB as status byte) 11D=285=**28.5%**  
02 FE 04 00 04 CB D1  
02 **FE 05 01 2F CB** FE <- Muscle%(05 3rd byte and CB as status byte) 12F=303=**30.3%**  
02 **FE 06 03 C1 CB** 93 <- KiloCal(06 3rd byte and CB as status byte) 3C1=961=**961**  
02 **FE 07 00 0E CB** DE <- Bone%(07 3rd byte and CB as status byte) E=14=**1.4%**  
02 **FE 08 02 0D CB** E0 <- Total Body Water(TBW)%(08 3rd byte and CB as status byte) 20D=525=**52.5%**  
02 FE 09 00 15 CB E7  
02 FE 0A 00 8E CB 61  
02 FE FC 00 00 CB C5  
02 FB 00 00 00 CB C6  
02 FA 00 01 02 CB C8  
02 FE 10 00 00 CC DA  
