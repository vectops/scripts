```
:local ovhddnsuser ""
:local ovhddnspass ""
:local theinterface ""
:local ovhddnshost ""
:local ipddns [:resolve $ovhddnshost]
:local ipfresh [ /ip address get [/ip address find interface=$theinterface ] address ]

:for i from=( [:len $ipfresh] - 1) to=0 do={ 
  :if ( [:pick $ipfresh $i] = "/") do={ 
  :set ipfresh [:pick $ipfresh 0 $i]
} 
 
:if ($ipddns != $ipfresh) do={
   :global str "nic/update?system=dyndns&hostname=$ovhddnshost&myip=$ipfresh&wildcard=OFF&backmx=NO&mx=NOCHG"

   /tool fetch address=www.ovh.com host=www.ovh.com src-path=$str mode=https user=$ovhddnsuser password=$ovhddnspass dst-path=("/ovhddns.".$ovhddnshost)
   :delay 1
   :global ovhresult [/file get "ovhddns.$ovhddnshost" contents]
   :global str [/file find name="ovhddns.$ovhddnshost"]
   /file remove $str
}
```
