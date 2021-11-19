# Version history of WeatherTag4Analytics

## v2 - 2021-11-22

### v2 general changes
* renamed tag from "Google Analytics Weather Tag Template" to "WeatherTag4Analytics
* Added FAQ section in [REAMDE.md]
* Updated installation guide in [REAMDE.md] with focus on CMP integration

### v2 functional changes
* renamed weather parameters
  * "status" to "detailed status"
  * "temperature" to "maximum temperature"
* added new weather parameters
  * grouped (weather) status (less than 10 different values compared to detailed weather status)
  * sunshine hours
  * minimun temperature
  * minumum windchill temperature
* Added new field "user consent"
  * this fields contains the name of a variable that must be true in order for the tag to send data to the backend. It has been introduced to simplify integration with a CMP

## v1 - initial release

The initial release support the four weather parameters
* status
* temperature
* precipitation
* windchill