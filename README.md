# Mikrotik-Address-List для Discord, Youtube, Instagram 
Листы брал здесь
https://github.com/vogster/Mikrotik-Address-List
Добавить таск в sheduler для mikrotik
```
/system scheduler add interval=2h name="update address-lists" on-event="local resource [toarray \
\"youtube,discord,instagram\"]; \\\r\
\n/ip firewall address-list remove [find list=\$list dynamic=no]; foreach \
list in=\$resource do={ \\\r\
\nlocal result [/tool fetch url=\"https://raw.githubusercontent.com/Discri\
menthium/Mikrotik-Address-List/refs/heads/main/\$list.list\" as-value outp\
ut=user]; \\\r\
\nlocal result [pick \$result 0]; \\\r\
\nlocal newresult; for i from=0 to=([len \$result]-1) do={ \\\r\
\nlocal tmp [pick \$result \$i]; \\\r\
\nif (\$tmp !=\"\\n\") do={ \\\r\
\nset newresult \"\$newresult\$tmp\" } }; \\\r\
\nlocal newresult [toarray \$newresult]; \\\r\
\nforeach value in=\$newresult do={/ip firewall/address-list/add address=\
\$value list=\$list} }" 
```
