```
:local dateTime ([ /system clock get date ] . " " . [ / system clock get time ]);

:local cfzoneid ""
:local cfdnsrecordid ""
:local cftoken ""
:local cfemail ""
:local cfdnshost ""

:local publicinterface ""
:local ipddns [:resolve $cfdnshost server=1.1.1.1]
:local ipfresh [ /ip address get [/ip address find interface=$publicinterface ] address ]
:set ipfresh [:pick $ipfresh 0 [:find $ipfresh "/" -1]]

:local cfurl "https://api.cloudflare.com/client/v4/zones/$cfzoneid/dns_records/$cfdnsrecordid"
:local cfDataDNS "{\"type\":\"A\",\"name\":\"$cfdnshost\",\"content\":\"$ipfresh\",\"ttl\":60,\"proxied\":false,\"comment\":\"last update: $dateTime\"}"
:local cfHeader "X-Auth-Email: $cfemail,Authorization: Bearer $cftoken,Content-Type: application/json"

:if ($ipddns != $ipfresh) do={
   /tool fetch url=$cfurl http-data=$cfDataDNS http-header-field=$cfHeader http-method=put keep-result=no
}
```
