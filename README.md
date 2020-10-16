### Smarten your gate release buzzer

#### Modify your Sonoff Mini DIY
... such that it acts as a pure switch instead of interconnecting mains power.  
For a howto showing modification of Sonoff Basic, check out https://www.youtube.com/watch?v=9IbP7dhyonw :-) 

#### Install
(TODO: Wiring diagram)

#### Add Sonoff Mini DIY to FHEM
##### Switch Config
```
defmod sonoffdiy HTTPMOD none 0
attr sonoffdiy userattr get01Data get01Method:GET,POST,PUT get01Name get01NoArg:0,1 get01URL reading01Name reading01Regex set01Data set01Name set01NoArg:0,1 set01URL set02Data set02Name set02NoArg:0,1 set02URL
attr sonoffdiy get01Data { "deviceid": "", "data": { } }
attr sonoffdiy get01Method POST
attr sonoffdiy get01Name info
attr sonoffdiy get01NoArg 1
attr sonoffdiy get01URL http://SONOFFDIYIPADDRESS:8081/zeroconf/info
attr sonoffdiy httpVersion 1.1
attr sonoffdiy reading01Name state
attr sonoffdiy reading01Regex "switch":"([\w\.]+)
attr sonoffdiy room Homekit
attr sonoffdiy set01Data { "deviceid": "", "data": { "switch": "off" } }
attr sonoffdiy set01Name off
attr sonoffdiy set01NoArg 1
attr sonoffdiy set01URL http://SONOFFDIYIPADDRESS:8081/zeroconf/switch
attr sonoffdiy set02Data { "deviceid": "", "data": { "switch": "on" } }
attr sonoffdiy set02Name on
attr sonoffdiy set02NoArg 1
attr sonoffdiy set02URL http://SONOFFDIYIPADDRESS:8081/zeroconf/switch
attr sonoffdiy timeout 2
```
##### State polling
```
defmod sonoffdiystate at +*00:00:03 get sonoffdiy info
```
##### Watchdog to turn switch off
```
defmod sonoffdiyw watchdog sonoffdiy:on.* 00:00:02 SAME set sonoffdiy off;; trigger sonoffdiyw .
```
