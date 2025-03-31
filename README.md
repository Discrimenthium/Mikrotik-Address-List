# Mikrotik-Address-List для Discord, Youtube, Instagram 
Листы брал здесь
https://github.com/vogster/Mikrotik-Address-List
  Исправил формат строк IP-адресов. 
Скрипт-однострочник добавляет задание "update address-lists" в sheduler. 
В массиве resource хранятся говорящие о себе "названия", которые соответствуют raw-страницам с этого репозитория.
remove удаляет все address-list записи мо маске *.list
Для каждого "названия" выполняется fetch команда, загружающая страницу в виде строки в переменную. Затем переменная очищается от ненужного выхлопа fetch/переносов строк и преобразовывается в массив ip-адресов. Далее для каждого $value добавляется запись в нужный address-list       
Sheduler task:
```
/system scheduler add interval=2h name="update address-lists" on-event="local resource [toarray \
    \"youtube,discord,instagram\"]; \\\r\
    \n/ip firewall address-list remove [find list~\"list\" dynamic=no]; foreac\
    h list in=\$resource do={ \\\r\
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
    \$value list=\"\$list.list\"} }"
```
