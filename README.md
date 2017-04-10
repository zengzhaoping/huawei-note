# huawei-note
ninja:
$ cmake -G Ninja // CMakeLists.txt --> build.inja
$ ninja  //build 
/**********************fastboot***********************************/
cls
setlocal enabledelayedexpansion
:start

set FASTBOOT_CMD=%cd%\fastboot.exe

@echo -----------------------------
@echo "Toronto Fastboot Updating......"

adb reboot bootloader

%FASTBOOT_CMD% getvar max-download-size 

%FASTBOOT_CMD%  flash partition          gpt_both0.bin
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash rpm                 rpm.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash tz                  tz.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash sbl1                sbl1.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash cmnlib              cmnlib.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash cmnlib64            cmnlib64.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash devcfg              devcfg.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash lksecapp            lksecapp.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash keymaster           keymaster.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash aboot               emmc_appsboot.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash mdtp                mdtp.img
	@if not 0==!ERRORLEVEL! goto error

if exist cust.img (
	%FASTBOOT_CMD%  flash cust               cust.img
		@if not 0==!ERRORLEVEL! goto error
)

%FASTBOOT_CMD%  flash modem               NON-HLOS.bin
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash dsp                 adspso.bin
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash boot                 boot.img
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash recovery             recovery.img
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash erecovery             erecovery.img
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash cache                cache.img
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash system               system.img
	@if not 0==!ERRORLEVEL! goto error
TIMEOUT /T 12 /NOBREAK
%FASTBOOT_CMD%  flash apdp               	apdp.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash msadp               msadp.mbn
	@if not 0==!ERRORLEVEL! goto error
%FASTBOOT_CMD%  flash userdata           userdata.img
	@if not 0==!ERRORLEVEL! goto error

%FASTBOOT_CMD%  flash vendor             vendor.img
@if errorlevel 1 goto error

%FASTBOOT_CMD%  flash product            product.img
@if errorlevel 1 goto error

%FASTBOOT_CMD% flash version          version.img
if errorlevel 1 goto error

@echo on

%FASTBOOT_CMD% reboot
@if not !ERRORLEVEL!==0 goto error

@goto sucess

:error

@echo Update FAIL!
@echo ===========================================
@echo       @@@@@@@   @@     @@@@   @@@@
@echo        @@  @@  @@@@     @@     @@
@echo        @@   @ @@  @@    @@     @@
@echo        @@     @@  @@    @@     @@
@echo        @@  @  @@  @@    @@     @@
@echo        @@@@@  @@  @@    @@     @@
@echo        @@  @  @@@@@@    @@     @@   @
@echo        @@     @@  @@    @@     @@  @@
@echo        @@     @@  @@    @@     @@  @@
@echo       @@@@    @@  @@   @@@@   @@@@@@@
@echo ===========================================
@echo 错误可能的原因：
@echo    1、保证所有镜像都存在，如果有"cannot load XXX.img: No error",表示XXX镜像不存在，请拷贝相应的镜像。
@echo    2、请确保所有的镜像加载都返回OK.如果返回Command not allowed字样,请到生产组服务器进行解锁。
@pause
EXIT /b  1

:sucess
@echo ===========================================
@echo                 @@@   @@@  @@
@echo                @@ @@   @@  @@
@echo               @@   @@  @@ @@
@echo               @@   @@  @@ @@
@echo               @@   @@  @@@@
@echo               @@   @@  @@ @@
@echo               @@   @@  @@ @@
@echo                @@ @@   @@  @@
@echo                 @@@   @@@  @@
@echo ===========================================
@echo Update Sucess, reboot......
@pause
EXIT /b  0
/**********************fastboot end***********************************/

