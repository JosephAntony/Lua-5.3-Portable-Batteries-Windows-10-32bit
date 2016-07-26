** WORK IN PROGRESS **


Lua windows plus C/non-C modules standalone help
-------------------------------

For this manual everything will be in c:\work folder, create it.

1. Installing MinGW32 suite with C/C++ compiler
	- download MinGW32 from here https://sourceforge.net/projects/mingw/files/Installer/
		- file name is mingw-get-setup.exe
	- run it, click install
	- change installation directory to c:\work\mingw
	- click "Continue"
	- at the end of the process click "Continue" again, new window opens where you can choose what to install
	- mark the following for installation
		- mingw-developer-toolkit
		- mingw32-base
		- mingw32-gcc-g++
		- msys-base
	- in window menu click on "Installation" -> "Apply changes"
	- new window opens, click "Apply", installation process begins
	- after end click "Close" and close also MinGW32 window.
	- navigate to C:\work\mingw\msys\1.0 and make shortcut from msys.bat file on the desktop we will use Msys for compilation
	- add C:\work\mingw\bin to your PATH variable if it is not there

2. Installing Git for Windows
	- download Git installation file from here https://git-scm.com/download/win
	- run it, click next, change directory to c:\work\git
	- click "Next" 3 times
	- check Use Git from the Windows Command Prompt, click "Next"
	- check Checkout Windows-style, ... click "Next"
	- check Use Windows' default console window, click "Next"
	- click "Install", drink coffee, blame Lua :)
	- Finish it
	
3. Compiling and installing Lua
	- download latest lua (5.3.3) from here https://www.lua.org/ftp/lua-5.3.3.tar.gz
	- unpack it to c:\work\src ... it is tar.gz archive you will need http://www.7-zip.org/download.html to unpack it
	- now run msys.bat via desktop icon
	- run "cd c:/work/src/lua-5.3.3" on the msys command line
	- run "make mingw" command
	- run "make local install" command, this creates folder c:\work\src\lua-5.3.3\install which contains you Lua installation
	- from c:\work\src\lua-5.3.3\src copy file lua53.dll into c:\work\src\lua-5.3.3\install\bin folder
	- copy whole folder "c:\work\src\lua-5.3.3\install" to c:\work ... rename it to "lua"
	- c:\work\lua now contains bin,include,lib,man,share folders.
	- you can try to run c:\work\lua\bin\lua.exe now :)
	- copy "libgcc_s_dw2-1.dll" file from "C:\work\mingw\bin" to c:\work\lua\bin - this is essential for standalone distribution
	- add C:\work\lua\bin to your PATH variable if you wish and if it is not there
	- congrats you compiled your very own Lua from sources
	
4. Installing Luarocks
	- here are original install instructions https://github.com/keplerproject/luarocks/wiki/Installation-instructions-for-Windows
	- download latest Luarocks (2.3.0) from here http://keplerproject.github.io/luarocks/releases/
	- create folder c:\work\src, unpack downloaded zip file in that folder
	- run command window .. Win+R, write "cmd" in input box, press Enter
	- in command window write "cd c:\work\src\luarocks-2.3.0-win32"
	- your command line now should look like this "c:\work\src\luarocks-2.3.0-win32>"
	- now run following command "install.bat /p c:\work\luarocks /tree c:\work\luarocks\systree /lv 5.3 /lua c:\work\lua /mw"
	- confirm privileges and installation start
	- press any key to close window
	- add "C:\work\luarocks" into your PATH variable
	- Luarocks is installed
	- close all black command windows and run new one to reload PATH settings
	
5. Installing first Lua C module "luautf8" into systree
	- luarocks install lua-utf8
	- done, module is installed in systree
	
6.	Installing "luasec" module into systree
	- luasocket module will be installed automatically with luasec module, don't bother
	- first install OpenSSL binary package from here https://slproweb.com/products/Win32OpenSSL.html to avoid compilation of sources, btw. it works with msys.
	- download full version, not light version, 32bit, I use this now "Win32 OpenSSL v1.0.1t"
	- run installer, install it anywhere (I use default "C:\OpenSSL-Win32"), if installer asks where to copy OpenSSL DLLs choose "The OpenSSL binaries(/bin) directory"
	- donate author if you wish and finish installation
	- in cmd window run the following command "luarocks install luasec OPENSSL_DIR=c:\OpenSSL-Win32 OPENSSL_LIBDIR=c:\OpenSSL-Win32\lib OPENSSL_INCDIR=c:\OpenSSL-Win32\include"
	- use your path in command, path where you installed openssl
	- luasec and luasocket are now installed in systree
	- for our standalone distro navigate to "c:\OpenSSL-Win32" and copy libeay32.dll, libssl32.dll, ssleay32.dll into "c:\work\lua\bin" folder
	- also copy "msvcr120.dll" from "c:\OpenSSL-Win32\bin" into c:\work\lua\bin
	
7.	Installing "luasql-mysql" module into systree
	- download MySQL Connector/C 32bit from here https://dev.mysql.com/downloads/connector/c/
	- run the installer, I installed it into "c:\work\src\connector"
	- now we need to edit module rockspec file because to this time there is error which library to link with Lua
	- download rockspec file here "https://luarocks.org/modules/tomasguisasola/luasql-mysql/cvs-1"
	- right click on "rockspec" link and choose "Save link as" and store it in "c:\work\src" folder
	- edit file with editor which can handle unix line-endings, say notepad++
	- on line 29 replace libraries = { "mysqlclient" }, with libraries = { "libmysql" },
	- in cmd window run "luarocks install c:\work\src\luasql-mysql-cvs-1.rockspec MYSQL_DIR=c:\work\src\connector MYSQL_INCDIR=c:\work\src\connector\include MYSQL_LIBDIR=c:\work\src\connector\lib"
	- copy file "libmysql.dll" from "C:\work\src\connector\lib" into "C:\work\lua\bin"
	- done luasql-mysql module is installed in systree
	
	
	
	