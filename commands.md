# Command list

!> If a command is not readable or writable then it can only be received from the SF40 and not sent to it.

|ID|Name|Description|RW|Read bytes|Write bytes|Persists|
|---|---|---|---|---|---|---|
|0	|[Product name](command_detail?id=product-name-0)		        	|Product name									|R	|16	|-	|-|
|1	|[Hardware version](command_detail?id=hardware-version-1)	    	|Hardware revision								|R	|4	|-	|-|
|2	|[Firmware version](command_detail?id=firmware-version-2)	    	|Firmware revision								|R	|4	|-	|-|
|3	|[Serial number](command_detail?id=serial-number-3)		    	|Serial number									|R	|16	|-	|-|
|7	|[UTF8 text message](command_detail?id=utf8-text-message-7)		|Human readable text message					|-	|-	|-	|-|
|9	|[User data](command_detail?id=user-data-9)				        |16 byte store for user data					|RW	|16	|16	|Y|
|10	|[Token](command_detail?id=token-10)					            |Next usable safety token      				    |R	|2	|-	|-|
|12	|[Save parameters](command_detail?id=save-parameters-12)		    |Store persistable parameters					|W	|-	|2	|-|
|14	|[Reset](command_detail?id=reset-14)       		                |Restart the unit            					|W	|-	|2	|-|
|16	|[Stage firmware](command_detail?id=stage-firmware-16)			    |Upload firmware file pages 					|RW	|4	|130 |-|
|17	|[Commit firmware](command_detail?id=commit-firmware-17)			|Apply staged firmware         					|RW	|4	|0	|-|
|20	|[Incoming voltage](command_detail?id=incoming-voltage-20)		    |Measured incoming voltage                      |R	|4	|-	|-|
|30	|[Stream](command_detail?id=stream-30)				                |Current data stream type						|RW	|4	|4	|N|
|48	|[Distance output](command_detail?id=distance-output-48)		        |Streaming distance data						|-	|-  |-	|-|
|50	|[Laser firing](command_detail?id=laser-firing-50)		     		|Laser firing state								|RW	|1	|1	|N|
|55	|[Temperature](command_detail?id=temperature-55)		     	  	|Measured temperature							|R	|4	|-	|-|
|90	|[Baud rate](command_detail?id=baud-rate-90)		            	|Serial baud rate								|RW	|1	|1	|Y|
|105|[Distance](command_detail?id=distance-105)		        	    |Single direction distance						    |RW	|12	|6	|N|
|106|[Motor state](command_detail?id=motor-state-106)		        |Motor state								    |R	|1	|-	|-|
|107|[Motor voltage](command_detail?id=motor-voltage-107)		        	|Measured motor voltage							|R	|2	|-	|-|
|108|[Output rate](command_detail?id=output-rate-108)	        |Streaming point output rate			        |RW	|1	|1	|Y|
|109|[Forward offset](command_detail?id=forward-offset-109)		    |Forward orientation offset						|RW	|2	|2	|Y|
|110|[Revolutions](command_detail?id=revolutions-109)		    |Number of revolutions	    		|R	|4	|-	|-|
|111|[Alarm state](command_detail?id=alarm-state-111)		    |Triggered state of alarms		    		|R	|1	|-	|-|
|112|[Alarm 1](command_detail?id=alarm-1-112)		            |Alarm 1 configuration					        |RW	|7	|7	|Y|
|113|[Alarm 2](command_detail?id=alarm-2-113)		            |Alarm 2 configuration					        |RW	|7	|7	|Y|
|114|[Alarm 3](command_detail?id=alarm-3-114)		            |Alarm 3 configuration					        |RW	|7	|7	|Y|
|115|[Alarm 4](command_detail?id=alarm-4-115)		            |Alarm 4 configuration					        |RW	|7	|7	|Y|
|116|[Alarm 5](command_detail?id=alarm-5-116)		            |Alarm 5 configuration					        |RW	|7	|7	|Y|
|117|[Alarm 6](command_detail?id=alarm-6-117)		            |Alarm 6 configuration					        |RW	|7	|7	|Y|
|118|[Alarm 7](command_detail?id=alarm-7-118)		            |Alarm 7 configuration					        |RW	|7	|7	|Y|
