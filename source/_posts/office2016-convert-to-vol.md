---
title: Office2016零售版转VOL版
date: 2019-11-25 16:17:05
tags: [office]
---

将下列文本另存为bat文件，运行即可：

```bat
@ECHO OFF

if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" (
    cd /d "%ProgramFiles%\Microsoft Office\Office16"
)
if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" (
    cd /d "%ProgramFiles(x86)%\Microsoft Office\Office16"
)

:WH
cls
echo ----------------------------------------------
echo 1. 零售版 Office Pro Plus 2016 转化为VOL版
echo 2. 零售版 Office Visio Pro 2016 转化为VOL版
echo 3. 零售版 Office Project Pro 2016 转化为VOL版
echo ----------------------------------------------

set /p tsk="请选择（1-3）: "
if not defined tsk goto:err
if %tsk%==1 goto:convert_office
if %tsk%==2 goto:convert_visio
if %tsk%==3 goto:convert_project

:err
goto :WH

:convert_office
cls
echo Convert Office2016
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
goto :end

:convert_visio
cls
echo Convert Visio2016
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_kms*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_mak*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
cscript ospp.vbs /inpkey:PD3PC-RHNGV-FXJ29-8JK7D-RJRJK
goto :end

:convert_project
cls
echo Convert Project2016
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_kms*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_mak*.xrm-ms') do (
    cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
cscript ospp.vbs /inpkey:YG9NW-3K39V-2T3HJ-93F3Q-G83KT
goto :end

:end
pause >nul
exit
```
