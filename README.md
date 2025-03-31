# Mikrotik-Address-List для Discord, Youtube, Instagram 
Листы брал здесь
https://github.com/vogster/Mikrotik-Address-List

  Исправил формат строк IP-адресов. Объединил множество IP в адреса с большей маской.
## Порядок работы
Скрипт-однострочник добавляет задание "update address-lists" в sheduler.
Таск выполняется каждые 2 часа, и содержит:
- В массиве resource хранятся говорящие о себе "названия", которые соответствуют raw-страницам с этого репозитория.
- Remove удаляет все address-list записи мо маске *.list.
- Для каждого "названия" выполняется fetch команда, загружающая страницу в виде строки в переменную. Затем переменная очищается от ненужного выхлопа fetch/переносов строк и преобразовывается в массив ip-адресов. 
- Для каждого элемента массива $value добавляется запись в нужный address-list       
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
