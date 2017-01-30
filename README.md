# Cisco-SPA-5XX-3XX-FusionPBX
Checklist of working features and documentation

## MWI

Works without additional configuration on a registered extension with voicemail box

## Call between extensions

Works without additional configuraiton between registered extensions in same domain

## Call Inbound/DID

Works, configure a Gateway (SIP Trunk) and an Inbound route which matches an incoming number and sends it somewhere

Also, make sure you have set your SIP provider to send calls to your-ip:5080 which is the SIP server for external calls

## Call Outbound/CID

Works, configure a Gateway, Outbound Route to match 10 digits and prefix 1, as well as 911,411, 311 etc. Configure outbound caller ID on per extension basis in extension settings

## Conferencing

Works both from the phone (ad-hoc) and also using the FusionPBX Conference Center app which can be used to setup moderator permissions, join announcements, etc. To set up Conference Center:
    
    Create Conference Center: Apps > Conference Center > +
    Create Conference Room: Apps > Conference Center > Rooms > +

## Speed dial + BLF

Works, use something like this in Cisco XML config
    
    <Line_Enable_2_ ua="na">No</Line_Enable_2_>
    <Extension_2_ ua="na">Disabled</Extension_2_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
    <Short_Name_2_ ua="na">Speed dial + BLF</Short_Name_2_>
    <Share_Call_Appearance_2_ ua="na">private</Share_Call_Appearance_2_> <!-- options: private/shared -->
    <Extended_Function_2_ ua="na">fnc=sd+blf;sub=101@mydomain.mysite.com</Extended_Function_2_>

## Call Flow + BLF (For Example: Day/Night Mode Toggle Button)

Works

First go to Apps > Call Flows > + to create:
       
       Name: Day/Night Mode
       Extension: 180
       Feature Code: *180
       Sound: ivr/ivr-disabled.wav
       Destination: (Ring Group, Queue, Day Mode IVR, or Extension here)
       Sound: ivr/ivr-enabled.wav
       Alternate Destination: Night Mode IVR
       SAVE

Cisco XML config example:

      <!-- Line Key 3 -->
      <Line_Enable_3_ ua="na">No</Line_Enable_3_>
      <Extension_3_ ua="na">Disabled</Extension_3_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
      <Short_Name_3_ ua="na"></Short_Name_3_>
      <Share_Call_Appearance_3_ ua="na">private</Share_Call_Appearance_3_> <!-- options: private/shared -->
      <Extended_Function_3_ ua="na">fnc=sd+blf;sub=*180@mydomain.mysite.com</Extended_Function_3_>

## Call Park + BLF for Slots

Works. There are a few different ways to use Call Park. In this example each phone will only have a line key for Slot 1 and Slot 2. To park a call, simply press the Slot 1/2 line key while on a call. The current call will be parked to that slot and the line key will be flashing red until the call is picked up. To pick up the call from another extension, press the flashing Slot 1/2 line key.

Cisco XML config example:

      <Extension_3_ ua="na">Disabled</Extension_3_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
      <Short_Name_3_ ua="na">Slot 1</Short_Name_3_>
      <Share_Call_Appearance_3_ ua="na">private</Share_Call_Appearance_3_> <!-- options: private/shared -->
      <Extended_Function_3_ ua="na">fnc=sd+blf+cp;sub=park+*5901@nationwide.locklinnetworks.com</Extended_Function_3_>

## Voicemail to Email

Works, configure email SMTP settings in Advanced > Default Settings > Email. Then configure voicemail to email address in extension settings

## IVR

Works, go to Dialplan > Dialplan Manager > Recordings. Change "pin_number=1928405" to your own pin. Dial \*732, enter your password, then enter recording ID numberlike 100.

When finished, go to Apps > IVR Menu > +. Setup the new IVR and you can use the recording made earlier.

## Sample Cisco SPA514G spa514G.xml located in /xml/customer1/

    <flat-profile>
    <Resync_On_Reset>Yes</Resync_On_Reset>
    <Resync_Periodic>10</Resync_Periodic>
    <Profile_Rule>http://mydomain.mysite.com/xml/customer1/$MA.xml</Profile_Rule>
    </flat-profile>
    
## Sample Cisco SPA514G mac-address.xml located in /xml/customer1/

    <flat-profile>

      <Server_Type ua="na">Asterisk</Server_Type>

      <!-- Line Configuration-->
      <Line_Enable_1_ ua="na">Yes</Line_Enable_1_>
      <Line_Enable_2_ ua="na">No</Line_Enable_2_>
      <Line_Enable_3_ ua="na">No</Line_Enable_3_>
      <Line_Enable_4_ ua="na">No</Line_Enable_4_>

       <!-- Line Key 1 -->
      <Extension_1_ ua="na">1</Extension_1_>
      <Short_Name_1_ ua="na">ext 100</Short_Name_1_>
      <Share_Call_Appearance_1_ ua="na">private</Share_Call_Appearance_1_> <!-- options: private/shared -->
      <Extended_Function_1_ ua="na"></Extended_Function_1_>

       <!-- Line Key 2 -->

      <Extension_2_ ua="na">Disabled</Extension_2_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
      <Short_Name_2_ ua="na"></Short_Name_2_>
      <Share_Call_Appearance_2_ ua="na">private</Share_Call_Appearance_2_> <!-- options: private/shared -->
      <Extended_Function_2_ ua="na"></Extended_Function_2_>

        <!-- Line Key 3 -->

      <Extension_3_ ua="na">Disabled</Extension_3_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
      <Short_Name_3_ ua="na"></Short_Name_3_>
      <Share_Call_Appearance_3_ ua="na">private</Share_Call_Appearance_3_> <!-- options: private/shared -->
      <Extended_Function_3_ ua="na"></Extended_Function_3_>

      <!-- Line Key 4 -->

      <Extension_4_ ua="na">Disabled</Extension_4_> <!-- options: 1/2/3/4/5/6/7/8/9/10/11/12/Disabled -->
      <Short_Name_4_ ua="na"></Short_Name_4_>
      <Share_Call_Appearance_4_ ua="na">private</Share_Call_Appearance_4_> <!-- options: private/shared -->
      <Extended_Function_4_ ua="na"></Extended_Function_4_>

      <!-- Proxy Configuration -->

      <Display_Name_1_ ua="na">Joe</Display_Name_1_>
      <User_ID_1_ ua="na">100</User_ID_1_>
      <Proxy_1_ ua="na">customer1.mysite.com</Proxy_1_>
      <Register_1_ ua="na">Yes</Register_1_>
      <Password_1_ ua="na">secretpass</Password_1_>
      <Use_Auth_ID_1_ ua="na">No</Use_Auth_ID_1_>
      <Dial_Plan_1_ ua="na">(*xxxxx*xxx|7xxx|1xxS0|[3469]11S0|911S0|[2-9]xxxxxxxxxS0)</Dial_Plan_1_>
      <Log_Missed_Calls_for_EXT_1 ua="na">Yes</Log_Missed_Calls_for_EXT_1>

      <Display_Name_2_ ua="na"></Display_Name_2_>
      <User_ID_2_ ua="na"></User_ID_2_>
      <Proxy_2_ ua="na"></Proxy_2_>
      <Register_2_ ua="na">Yes</Register_2_>
      <Password_2_ ua="na"></Password_2_>
      <Use_Auth_ID_2_ ua="na">No</Use_Auth_ID_2_>
      <Dial_Plan_2_ ua="na">(*xxxxx*xxx|7xxx|1xxS0|[3469]11S0|911S0|[2-9]xxxxxxxxxS0)</Dial_Plan_2_>
      <Log_Missed_Calls_for_EXT_2 ua="na">Yes</Log_Missed_Calls_for_EXT_2>

      <Display_Name_3_ ua="na"></Display_Name_3_>
      <User_ID_3_ ua="na"></User_ID_3_>
      <Proxy_3_ ua="na"></Proxy_3_>
      <Register_3_ ua="na">Yes</Register_3_>
      <Password_3_ ua="na"></Password_3_>
      <Use_Auth_ID_3_ ua="na">No</Use_Auth_ID_3_>
      <Dial_Plan_3_ ua="na">(*xxxxx*xxx|7xxx|1xxS0|[3469]11S0|911S0|[2-9]xxxxxxxxxS0)</Dial_Plan_3_>
      <Log_Missed_Calls_for_EXT_3 ua="na">Yes</Log_Missed_Calls_for_EXT_3>

      <Display_Name_4_ ua="na"></Display_Name_4_>
      <User_ID_4_ ua="na"></User_ID_4_>
      <Proxy_4_ ua="na"></Proxy_4_>
      <Register_4_ ua="na">Yes</Register_4_>
      <Password_4_ ua="na"></Password_4_>
      <Use_Auth_ID_4_ ua="na">No</Use_Auth_ID_4_>
      <Dial_Plan_4_ ua="na">(*xxxxx*xxx|7xxx|1xxS0|[3469]11S0|911S0|[2-9]xxxxxxxxxS0)</Dial_Plan_4_>
      <Log_Missed_Calls_for_EXT_4 ua="na">Yes</Log_Missed_Calls_for_EXT_4>

      <!-- Softkey Configuration -->

      <Programmable_Softkey_Enable ua="na">Yes</Programmable_Softkey_Enable>
      <Idle_Key_List ua="na">em_login|1;acd_login|1;acd_logout|1;astate|2;avail|3;unavail|3;redial|5;dir|6;cfwd|7;dnd|8;pickup|10;gpickup|11;unpark|12;guestIn|13;guestOut|13;em_logout</Idle_Key_List>
      <Missed_Call_Key_List ua="na">miss|4</Missed_Call_Key_List>
      <Off_Hook_Key_List ua="na">redial|1;dir|2;cfwd|3</Off_Hook_Key_List>
      <Dialing_Input_Key_List ua="na">dial|1;delchar|2;cancel|3;dir|4</Dialing_Input_Key_List>
      <Progressing_Key_List ua="na">endcall|4</Progressing_Key_List>
      <Connected_Key_List ua="na">xfer|1;bxfer|2;conf|3;endcall|4;toggle;confLx;xferLx;park;phold;flash;</Connected_Key_List>
      <Start-Xfer_Key_List ua="na">hold|1;endcall|2;xfer|4;toggle;</Start-Xfer_Key_List>
      <Start-Conf_Key_List ua="na">hold|1;endcall|2;conf|4;toggle;</Start-Conf_Key_List>
      <Conferencing_Key_List ua="na">hold|1;endcall|2;join|4</Conferencing_Key_List>
      <Releasing_Key_List ua="na">endcall|4;</Releasing_Key_List>
      <Hold_Key_List ua="na">resume|1;endcall|2;newcall|3;redial;dir;cfwd;dnd</Hold_Key_List>
      <Ringing_Key_List ua="na">answer|1;ignore|2;toggle|4</Ringing_Key_List>
      <Shared_Active_Key_List ua="na">newcall|1;barge|2;cfwd|3;dnd|4</Shared_Active_Key_List>
      <Shared_Held_Key_List ua="na">resume|1;barge|2;cfwd|3;dnd|4</Shared_Held_Key_List>
      <PSK_1 ua="na"></PSK_1>
      <PSK_2 ua="na"></PSK_2>
      <PSK_3 ua="na"></PSK_3>
      <PSK_4 ua="na"></PSK_4>
      <PSK_5 ua="na"></PSK_5>
      <PSK_6 ua="na"></PSK_6>
      <PSK_7 ua="na"></PSK_7>
      <PSK_8 ua="na"></PSK_8>
      <PSK_9 ua="na"></PSK_9>
      <PSK_10 ua="na"></PSK_10>
      <PSK_11 ua="na"></PSK_11>
      <PSK_12 ua="na"></PSK_12>
      <PSK_13 ua="na"></PSK_13>
      <PSK_14 ua="na"></PSK_14>
      <PSK_15 ua="na"></PSK_15>
      <PSK_16 ua="na"></PSK_16>

      <!-- Misc Configuration -->

      <Line_Navigation ua="na">Per Call</Line_Navigation>
      <Admin_Passwd ua="na">admin-web-pass</Admin_Passwd>
      <Signaling_Protocol ua="na">SIP</Signaling_Protocol>
      <Back_Light_Timer ua="na">Always On</Back_Light_Timer>
      <Station_Name ua="na">Joe</Station_Name>
      <Station_Display_Name ua="na">Joe</Station_Display_Name>
      <Voice_Mail_Number ua="na">*97</Voice_Mail_Number>
      <Time_Zone ua="na">GMT-06:00</Time_Zone>
      <Daylight_Saving_Time_Rule ua="na">start=3/8/7/2:0:0;end=11/1/7/2:0:0;save=1</Daylight_Saving_Time_Rule>
      <Daylight_Saving_Time_Enable ua="na">Yes</Daylight_Saving_Time_Enable>
      <Locale ua="na">en-US</Locale>
      <Softkey_Navigation_Style ua="na">More</Softkey_Navigation_Style>

      <!-- Network Configuration -->

      <NTP_Enable ua="na">Yes</NTP_Enable>
      <Primary_NTP_Server ua="na">us.pool.ntp.org</Primary_NTP_Server>
      <Secondary_NTP_Server ua="na">pool.ntp.org</Secondary_NTP_Server>
      <Provision_Enable ua="na">Yes</Provision_Enable>
      <Resync_Periodic ua="na">120</Resync_Periodic>
      <Transport_Protocol ua="na">http</Transport_Protocol>
      <Upgrade_Enable ua="na">Yes</Upgrade_Enable>
      <Upgrade_Rule ua="na">( $SWVER lt 7.6.2.a )? http://pbx.mysite.com/fw/spa51x-7-6-2a.bin</Upgrade_Rule>
      <Debug_Server ua="na">my-sys-log-server-ip</Debug_Server>
      <Debug_Level ua="na">1</Debug_Level> <!-- options: 0/1/2/3 -->
      <Syslog_Server ua="na">my-debug-server-ip</Syslog_Server>

      <!-- Code Configuration -->

      <Call_Return_Code ua="na"></Call_Return_Code>
      <Blind_Transfer_Code ua="na"></Blind_Transfer_Code>
      <Call_Back_Act_Code ua="na"></Call_Back_Act_Code>
      <Call_Back_Deact_Code ua="na"></Call_Back_Deact_Code>
      <Cfwd_All_Act_Code ua="na"></Cfwd_All_Act_Code>
      <Cfwd_All_Deact_Code ua="na"></Cfwd_All_Deact_Code>
      <Cfwd_Busy_Act_Code ua="na"></Cfwd_Busy_Act_Code>
      <Cfwd_Busy_Deact_Code ua="na"></Cfwd_Busy_Deact_Code>
      <Cfwd_No_Ans_Act_Code ua="na"></Cfwd_No_Ans_Act_Code>
      <Cfwd_No_Ans_Deact_Code ua="na"></Cfwd_No_Ans_Deact_Code>
      <CW_Act_Code ua="na"></CW_Act_Code>
      <CW_Deact_Code ua="na"></CW_Deact_Code>
      <CW_Per_Call_Act_Code ua="na"></CW_Per_Call_Act_Code>
      <CW_Per_Call_Deact_Code ua="na"></CW_Per_Call_Deact_Code>
      <Block_CID_Act_Code ua="na"></Block_CID_Act_Code>
      <Block_CID_Deact_Code ua="na"></Block_CID_Deact_Code>
      <Block_CID_Per_Call_Act_Code ua="na"></Block_CID_Per_Call_Act_Code>
      <Block_CID_Per_Call_Deact_Code ua="na"></Block_CID_Per_Call_Deact_Code>
      <Block_ANC_Act_Code ua="na"></Block_ANC_Act_Code>
      <Block_ANC_Deact_Code ua="na"></Block_ANC_Deact_Code>
      <DND_Act_Code ua="na"></DND_Act_Code>
      <DND_Deact_Code ua="na"></DND_Deact_Code>
      <Secure_All_Call_Act_Code ua="na"></Secure_All_Call_Act_Code>
      <Secure_No_Call_Act_Code ua="na"></Secure_No_Call_Act_Code>
      <Secure_One_Call_Act_Code ua="na"></Secure_One_Call_Act_Code>
      <Secure_One_Call_Deact_Code ua="na"></Secure_One_Call_Deact_Code>
      <Paging_Code ua="na"></Paging_Code>
      <Call_Park_Code ua="na"></Call_Park_Code>
      <Call_Pickup_Code ua="na"></Call_Pickup_Code>
      <Call_UnPark_Code ua="na"></Call_UnPark_Code>
      <Group_Call_Pickup_Code ua="na"></Group_Call_Pickup_Code>
      <Media_Loopback_Code ua="na"></Media_Loopback_Code>
      <Referral_Services_Codes ua="na"></Referral_Services_Codes>
      <Feature_Dial_Services_Codes ua="na"></Feature_Dial_Services_Codes>
      <Service_Annc_Base_Number ua="na"></Service_Annc_Base_Number>
      <Service_Annc_Extension_Codes ua="na"></Service_Annc_Extension_Codes>
      <Prefer_G711u_Code ua="na"></Prefer_G711u_Code>
      <Force_G711u_Code ua="na"></Force_G711u_Code>
      <Prefer_G711a_Code ua="na"></Prefer_G711a_Code>
      <Force_G711a_Code ua="na"></Force_G711a_Code>
      <Prefer_G723_Code ua="na"></Prefer_G723_Code>
      <Force_G723_Code ua="na"></Force_G723_Code>
      <Prefer_G722_Code ua="na"></Prefer_G722_Code>
      <Force_G722_Code ua="na"></Force_G722_Code>
      <Prefer_L16_Code ua="na"></Prefer_L16_Code>
      <Force_L16_Code ua="na"></Force_L16_Code>
      <Prefer_AMR-WB_Code ua="na"></Prefer_AMR-WB_Code>
      <Force_AMR-WB_Code ua="na"></Force_AMR-WB_Code>
      <Prefer_G726r16_Code ua="na"></Prefer_G726r16_Code>
      <Force_G726r16_Code ua="na"></Force_G726r16_Code>
      <Prefer_G726r24_Code ua="na"></Prefer_G726r24_Code>
      <Force_G726r24_Code ua="na"></Force_G726r24_Code>
      <Prefer_G726r32_Code ua="na"></Prefer_G726r32_Code>
      <Force_G726r32_Code ua="na"></Force_G726r32_Code>
      <Prefer_G726r40_Code ua="na"></Prefer_G726r40_Code>
      <Force_G726r40_Code ua="na"></Force_G726r40_Code>
      <Prefer_G729a_Code ua="na"></Prefer_G729a_Code>
      <Force_G729a_Code ua="na"></Force_G729a_Code>

    </flat-profile>
