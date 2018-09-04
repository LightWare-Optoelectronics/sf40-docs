# Command list

!> If a command is not readable or writable then it can only be received from the SF40 and not sent to it.

|ID|Name|Description|RW|Read bytes|Write bytes|Persists|
|---|---|---|---|---|---|---|
|0	|[Product name](command_detail?id=_0-product-name)		        	|Product name									|R	|16	|-	|-|
|1	|[Hardware version](command_detail?id=_1-hardware-version)	    	|Hardware revision								|R	|4	|-	|-|
|2	|[Firmware version](command_detail?id=_2-firmware-version)	    	|Firmware revision								|R	|4	|-	|-|
|3	|[Serial number](command_detail?id=_3-serial-number)		    	|Serial number									|R	|16	|-	|-|
|7	|[UTF8 text message](command_detail?id=_7-utf8-text-message)		|Human readable text message					|-	|-	|-	|-|
|9	|[User data](command_detail?id=_9-user-data)				        |16 byte store for user data					|RW	|16	|16	|Y|
|10	|[Token](command_detail?id=_10-token)					            |Next usable safety token      				    |R	|2	|-	|-|
|12	|[Save parameters](command_detail?id=_12-save-parameters)		    |Store persistable parameters					|W	|-	|2	|-|
|14	|[Reset](command_detail?id=_14-reset)       		                |Restart the unit            					|W	|-	|2	|-|
|16	|[Stage firmware](command_detail?id=_16-stage-firmware)			    |Upload firmware file pages 					|RW	|4	|130 |-|
|17	|[Commit firmware](command_detail?id=_17-commit-firmware)			|Apply staged firmware         					|RW	|4	|0	|-|
|20	|[Incoming voltage](command_detail?id=_20-incoming-voltage)		    |Measured incoming voltage                      |R	|4	|-	|-|
|30	|[Stream](command_detail?id=_30-stream)				                |Current data stream type						|RW	|4	|4	|N|
|48	|[Distance output](command_detail?id=_44-distance-data)		        |Streaming distance data						|R	|*varies*|-	|-|
|50	|[Laser firing](command_detail?id=_50-laser-firing)		     		|Laser firing state								|RW	|1	|1	|N|
|55	|[Temperature](command_detail?id=_55-temperature)		     	  	|Measured temperature							|R	|4	|-	|-|
|90	|[Baud rate](command_detail?id=_90-baud-rate)		            	|Serial baud rate								|RW	|1	|1	|Y|
|105|[Distance](command_detail?id=_91-i2c-address)		        	    |Single direction distance						    |RW	|10	|4	|N|
|106|[Motor state](command_detail?id=_93-measurement-mode)		        |Motor state								    |R	|1	|-	|-|
|107|[Motor voltage](command_detail?id=_94-zero-offset)		        	|Measured motor voltage							|R	|2	|-	|-|
|108|[Output rate](command_detail?id=_95-lost-signal-counter)	        |Streaming point output rate			        |RW	|1	|1	|Y|
|109|[Forward offset](command_detail?id=_96-alarm-a-distance)		    |Forward orientation offset						|RW	|2	|2	|Y|
|110|[Revolutions](command_detail?id=_97-alarm-b-distance)		    |Number of revolutions	    		|R	|4	|-	|-|
|111|[Alarm state](command_detail?id=_98-alarm-hysteresis)		    |Triggered state of alarms		    		|R	|1	|-	|-|
|112|[Alarm 1](command_detail?id=_121-servo-connected)		            |Alarm 1 configuration					        |RW	|7	|7	|Y|
|113|[Alarm 2](command_detail?id=_121-servo-connected)		            |Alarm 2 configuration					        |RW	|7	|7	|Y|
|114|[Alarm 3](command_detail?id=_121-servo-connected)		            |Alarm 3 configuration					        |RW	|7	|7	|Y|
|115|[Alarm 4](command_detail?id=_121-servo-connected)		            |Alarm 4 configuration					        |RW	|7	|7	|Y|
|116|[Alarm 5](command_detail?id=_121-servo-connected)		            |Alarm 5 configuration					        |RW	|7	|7	|Y|
|117|[Alarm 6](command_detail?id=_121-servo-connected)		            |Alarm 6 configuration					        |RW	|7	|7	|Y|
|118|[Alarm 7](command_detail?id=_121-servo-connected)		            |Alarm 7 configuration					        |RW	|7	|7	|Y|
