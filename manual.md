** DRAFT **

#Installing Lua 5.3.3 and C modules from sources on Windows
July 26, 2016

---

In this humble step-by-step manual we will learn how to compile and install Lua, Lua C modules and one pure Lua module. We will learn what to download, install and setup to achieve this.  

---

At the end of the following steps you will have standalone Lua distribution for Windows which:
  - runs on Windows 7, 8, 8.1, 10 ... 32/64 bit  (if you can run it on XP let me know :)
  - doesn't require from its user to install any additional libraries, runtimes, frameworks into his operating system to run correctly    
  - you can distribute it to your friends, collegues or clients with your scripts as a zip archive  
  - you can take it with you on USB key  

---

In this manual everything goes into **c:\work** folder, so please create it.

---


**1. Installing MinGW32 C/C++ compiler**
	- download MinGW32 from here https://sourceforge.net/projects/mingw/files/Installer/
		- file name is ```mingw-get-setup.exe```
	- run it, then click ```Install```
	- change installation directory to ```c:\work\mingw```
	- click ```Continue```
	- at the end of the process click ```Continue``` again, new window opens where you can choose which packages to install
	- mark the following for installation
		- mingw-developer-toolkit
		- mingw32-base
		- mingw32-gcc-g++
		- msys-base
	- in window menu click on *Installation* -> *Apply changes*
	- new window opens, click *Apply* and installation process starts
	- at the end click *Close* and close also MinGW32 window.
	- navigate to ```C:\work\mingw\msys\1.0``` folder and make shortcut from ```msys.bat``` file on the desktop, we will use Msys for compilation
	- add ```C:\work\mingw\bin``` folder to your ```PATH``` variable if it is not there

**2. Installing Git for Windows**
	- download Git installation file from here https://git-scm.com/download/win
	- run it, click *Next*, change directory to ```c:\work\git```
	- click *Next* 3 times
	- check *Use Git from the Windows Command Prompt*, click *Next*
	- check *Checkout Windows-style, ...* click *Next*
	- check *Use Windows' default console window*, click *Next*
	- click *Install*, drink coffee
	- finish it
	
**3. Compiling and installing Lua**
	- download latest lua (5.3.3 currently) from here https://www.lua.org/ftp/lua-5.3.3.tar.gz
	- create folder ```c:\work\src```
	- unpack it to ```c:\work\src``` ... it is tar.gz archive you will need something like http://www.7-zip.org/download.html to unpack it
	- now run ```msys.bat``` via desktop icon
	- run ```cd c:/work/src/lua-5.3.3``` on the msys command line
	- run ```make mingw``` command
	- run ```make local install``` command, this creates folder ```c:\work\src\lua-5.3.3\install``` which contains you Lua installation
	- from ```c:\work\src\lua-5.3.3\src``` copy file ```lua53.dll``` into ```c:\work\src\lua-5.3.3\install\bin``` folder
	- copy whole folder ```c:\work\src\lua-5.3.3\install``` to ```c:\work``` ... rename it to ```lua```
	- ```c:\work\lua``` now contains ```bin,include,lib,man,share``` folders.
	- you can try to run ```c:\work\lua\bin\lua.exe``` now :)
	- copy ```libgcc_s_dw2-1.dll``` file from ```C:\work\mingw\bin``` folder into ```c:\work\lua\bin``` folder - this is essential for standalone distribution
	- add ```C:\work\lua\bin``` to your ```PATH``` variable if you wish and if it is not there
	- **congrats you compiled your very own Lua from sources**
	
**4. Installing Luarocks**
	- here are original install instructions https://github.com/keplerproject/luarocks/wiki/Installation-instructions-for-Windows
	- download latest Luarocks (2.3.0) from here http://keplerproject.github.io/luarocks/releases/
	- unpack downloaded zip file in ```c:\work\src``` folder
	- run cmd window .. press ```Win+R``` write ```cmd``` in the input box, press ```Enter```
	- in cmd window commandline write `cd c:\work\src\luarocks-2.3.0-win32` press ```Enter```
	- your command line now should look like this ```c:\work\src\luarocks-2.3.0-win32>```
	- now run following command ```install.bat /p c:\work\luarocks /tree c:\work\luarocks\systree /lv 5.3 /lua c:\work\lua /mw```
	- confirm privileges and start of the installation process
	- when it ends up press any key to close window
	- add ```C:\work\luarocks``` into your ```PATH``` variable
	- Luarocks is installed
	- close all black cmd windows and run new one to reload ```PATH``` settings :)
	
**5. Installing first Lua C module *luautf8* into systree**
	- run command ```luarocks install lua-utf8``` in cmd window ... Zzzz ...
	- ... and module is installed in systree
	
**6. Installing *luasec* C module into systree**
	- luasocket module will be installed automatically with luasec module
	- first install OpenSSL binary package from here https://slproweb.com/products/Win32OpenSSL.html to avoid compilation of sources, btw. it works with msys so if you wish you can compile it from sources
	- download full version, not *light* version, 32bit, I use this now *Win32 OpenSSL v1.0.1t*
	- run installer, install it into ```c:\work\src\ssl```, if the installer asks you where to copy OpenSSL DLLs choose ```The OpenSSL binaries(/bin) directory```
	- donate author if you wish and finish installation
	- in cmd window run the following command ```luarocks install luasec OPENSSL_DIR=c:\work\src\ssl OPENSSL_LIBDIR=c:\work\src\ssl\lib OPENSSL_INCDIR=c:\work\src\ssl\include```
	- use your path in command above, path where you installed openssl
	- luasec and luasocket are now installed in systree
	- for our standalone distro navigate to ```c:\OpenSSL-Win32``` folder and copy ```libeay32.dll, libssl32.dll, ssleay32.dll``` files into ```c:\work\lua\bin``` folder
	- also copy ```msvcr120.dll``` file from ```c:\OpenSSL-Win32\bin``` folder into ```c:\work\lua\bin``` folder
	
**7. Installing *luasql-mysql* C module into systree**
	- download MySQL Connector/C 32bit from here https://dev.mysql.com/downloads/connector/c/
	- run the installer, I installed it into ```c:\work\src\connector```
	- now we need to edit module rockspec file because to this time there is error which library to link to create Lua module on Windows
	- download rockspec file here https://luarocks.org/modules/tomasguisasola/luasql-mysql/cvs-1
	- right click on ```rockspec``` link and choose ```Save link as``` and store it in ```c:\work\src``` folder
	- edit file with editor which can handle unix line-endings, say notepad++
	- on line 29 replace ```libraries = { "mysqlclient" }```, with ```libraries = { "libmysql" }```,
	- in cmd window run ```luarocks install c:\work\src\luasql-mysql-cvs-1.rockspec MYSQL_DIR=c:\work\src\connector MYSQL_INCDIR=c:\work\src\connector\include MYSQL_LIBDIR=c:\work\src\connector\lib```
	- copy file ```libmysql.dll``` from ```C:\work\src\connector\lib``` folder into ```C:\work\lua\bin``` folder
	- luasql-mysql module is installed in systree
	
	
**8. Installing *lsqlite3* C module into systree**
	- download rock file directly from SQLite http://lua.sqlite.org/index.cgi/index version 0.9.3 and unpack it into ```c:\work\src``` folder
	- download sqlite source http://sqlite.org/2016/sqlite-amalgamation-3130000.zip and unpack it into ```c:\work\src\sqlite``` folder
	- download sqlite precompiled library http://sqlite.org/2016/sqlite-dll-win32-x86-3130000.zip and unpack it also into ```c:\work\src\sqlite``` folder
	- open cmd window and run the following command ```luarocks install c:\work\src\lsqlite3_fsl09w\lsqlite3-0.9.3-0.rockspec SQLITE_DIR=c:\work\src\sqlite SQLITE_INCDIR=c:\work\src\sqlite SQLITE_LIBDIR=c:\work\src\sqlite```
	- copy ```sqlite3.dll``` file from ```c:\work\src\sqlite``` folder into ```c:\work\lua\bin``` folder
	- lsqlite3 module is installed in systree
	
**9. Installing *penlight* pure Lua module**
	- this is a bonus :) pure lua module, it is ideal resource on how to write code in lua, is well documented, full of examples
	- open cmd window and run the following command ```luarocks install penlight```
	- it is dependent on module *luafilesystem* which installs automaticaly don't worry
	- penlight module is installed in systree
	
**10. Final steps**
	- copy Lua binary modules from ```C:\work\luarocks\systree\lib\lua\5.3``` folder into ```C:\work\lua\lib\lua\5.3``` folder
	- copy Lua modules from ```C:\work\luarocks\systree\share\lua\5.3``` folder into ```C:\work\lua\share\lua\5.3``` folder
	- for your standalone distribution don't forget repeat previous two steps everytime you install/remove module into/from Luarocks systree .. make a batch file to automate it :)
	- of course you can add your Luarocks' systree subfolders into your ```LUA_PATH``` and ```LUA_CPATH``` variables but I don't do/need this
	- now you have your own distribution of Lua with some very usefull C modules, everything in ```c:\work\lua``` folder
	- test it for yourself, start lua interpreter in cmd window and try to load module to view its members
	- ```local u = require('lua-utf8') for k, v in pairs(u) do print(k,v) end```
	- ... and enjoy the Lua programming :)
	
---

**Some principles as far as I understand them**
  - if module is written in pure Lua it goes to your LUA_PATH, in our case into ```C:\work\lua\share\lua\5.3``` folder
  - if module is written in C and **has no** external (DLL/header files) dependencies resulting DLLs goes in LUA_CPATH in our case into ```C:\work\lua\lib\lua\5.3``` folder ... eg. luautf8 module installation.
  - if module is written in C and **has** external third party dependencies resulting DLL(s) goes in LUA_CPATH in our case into ```C:\work\lua\lib\lua\5.3``` folder and DLL(s) from third party which were linked to create resulting Lua DLL(s) goes into ```c:\work\lua\bin``` directory in our case ... eg. lsqlite3 module installation. 
  - in case of OpenSSL, third party DLLs ```libeay32.dll, libssl32.dll, ssleay32.dll``` were linked also with msvcr120.dll (Visual C++ redistributable library I think) so this file must also go into Lua's \bin folder.
  - If you install lsqlite3 module and look at the output from Luarocks (C linker) in cmd window you can see on third line from bellow the following:
```
mingw32-gcc -shared -o lsqlite3.dll lsqlite3.o -Lc:\work\src\sqlite\ -lsqlite3 c:/work/lua/bin/lua53.dll -lm
```
  - lsqlite3.dll is resulting Lua binary module which goes to LUA_CPATH
  - -lsqlite3 means sqlite3.dll (-l is linker switch) from sqlite distribution which we downloaded precompiled and goes to Lua's \bin folder
  - in case of other modules it is similar, you can deduct what to copy into Lua's bin folder from C linker output.
