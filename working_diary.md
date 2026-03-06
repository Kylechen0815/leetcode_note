# 3/5 

MTK Flash tool 

NAS路徑: smb://amt-nas.local/file/MTK/tools 

  

Linux版,需要調整執行權限chmod +x 

    SP_Flash_Tool_Selector_exe_Linux_v1.2228.00.100 (舊版) 

    SP_Flash_Tool_Selector_exe_Linux_v1.2548.00.100 (新版)      

  

windows版 

    SP_Flash_Tool_Selector_v1.2433.04.00 

 

 

Image paths：amt-nas.local/image/advantech_image/ 

Test case: advantech-mt8391-aim-79h-userdebug-2026-0225-1549 

  

Flash_tool execution: 

Stay in file :SP_Flash_Tool_Selector_exe_Linux_v1.2548.00.100 

Terminal command:   (will be rejected if typing   chmod +x *  due to no permission) 

sudo chmod +x * -R                            

          sudo ./FlashToolSelector 

    

  3.   Test case unzip 

       Run /media/yj-jing/Kyle/Project/tools/SP_Flash_Tool_Selector_exe_Linux_v1.2548.00.100/SP_Flash_Tool_V6 
  4.   Download and restart: 

         Click auto reboot and format all + download 

-------------------------------------------------------------------------------- 

  

  How to enable adb in android user build 

 

Enable Developer options: 

Open the Settings app. 

Scroll down and tap About phone (or About device) 

Find the Build number entry and tap it rapidly seven times until a message appears saying, "You are now a developer!". 

 

Enable USB debugging (or Wireless debugging): 

 System > Advanced >Developer options. 

 

Terminal command:   

Connect to device>  enter the device  

  adb  device  

  adb  shell 
  
  exit
----------------------------------------------------------------


ctrl + alt + t : 新的terminal視窗
 

 



 

 
