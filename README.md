# Mikrotik-Address-List для Discord, Youtube, Instagram 
Листы брал здесь
https://github.com/vogster/Mikrotik-Address-List
Пример скрипта для mikrotik
```
/ip firewall address-list remove [find list~"youtube.list"]
local list youtube.list; \
local result [/tool fetch url="https://raw.githubusercontent.com/Discrimenthium/ip-lists/refs/heads/main/$list" as-value output=user]; \
local result [pick $result 0]; \
local newresult; for i from=0 to=([len $result]-1) do={ \
local tmp [pick $result $i]; \
if ($tmp !="\n") do={ \
set newresult "$newresult$tmp" } }; \
local newresult [toarray $newresult]; \
foreach value in=$newresult do={/ip firewall/address-list/add address=$value list=$list}
```
