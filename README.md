# WeatherTag4Analytics

## Features 

The `meteonomiqs - weather tag` allows you to enrich your Google Analytics user sessions 
with the user's local weather conditions!

The version history can be found here: [VERSIONS.md]

## Setup

###  Step 1: Register at meteonomiqs.com

To use the Meteonomiqs Weather Tag, a registration is required at https://www.meteonomiqs.com/de/wetter-analytics/.

### Step 2: Import Template from the Google Tag manager's community template gallery

Open your Google Tag manager account and click on `Templates`.

Click on `Search Gallery` and search for `meteonomiqs - weather tag` from the solution gallery.

Click on the template and add to workspace.

![Manual upload](doc/images/choosetemplate.png "Import Template")

Click on add.

![Manual upload](doc/images/addtemplate.png "Import Template")

### Step 3: Data Privacy Statement & CMP configuration

What we need consent for:
* We store a cookie (`_sessmetonmq`) with value `true` for 30 minutes in order to send the request not more than once per 30 minutes to the meteonomiqs backend
* The request that is sent to the meteonomiqs backend includes the Google client id from the Google Analytics cookie `_ga`
* From the request, the IP address is used to derive the location (latitude, longitude) for determining the weather at that location
* The Google client id together with the weather data is sent to Google analyics
* IP address and location are not stored for further processing, but can be logged for troubleshooting. Logs are kept up to 10 days

An easy way is to add wetter.com Gmbh (meteonomiqs is a brand of wetter.com GmbH) as non IAB Vendor to your CMP (TCF2.0).

* Description: **The presumable location is determined based on your IP address. A weather query is then made with the location data. This weather data is transmitted to the website operator for analysis purposes. The IP address is stored in the logs to identify abuse for up to 10 days. No further processing takes place.**
* Name of processing company: **meteonomiqs.com  / wetter.com GmbH**
* Address of processing company: **Reichenaustr. 19a, 78467 Konstanz**
* Puposes: **Weather analytics**
* Data Collected: **Location based on the IP address**
* Technologies Used: **Cookies**
* Cookie URL: **-**
* Location of Processing: **European Union**
* Retention Period
  * **Cookie: 30 minutes**
  * **IP address in the log files for 10 days - the location and weather data is transmitted to the customer system, no further storage takes place on our site.**
* Policy of Procesor : **Data privacy https://www.meteonomiqs.com/data-privacy/**
* Data Protection Officer:  **datenschutz@wetter.com**
* Storage information: **Cookie (Maximum age of cookie storage: 30 minutes) is set through Weathertag but it is shown as a first Party. No Non-cookie storage**

If you are using a CMP prior to TCF2.0 or some other consent solution, please include the above information in your privacy statement as needed.

### Step 4: Create Variables

Go to 'Variables' on your Tag manager account and create data layer variables for both Google Analytics and wetter.com from your CMP. Name them as 'CMP.GoogleAnalytics' and 'CMP.WeatherTag' respectively.

Create another data layer variable and name it as 'dlv - mtqfired' for example as shown below. Fill in the data layer variable name as 'mtqfired'.

![Tag Configuration](doc/images/mtqfired.png "Tag Configuration")


### Step 5: Configure Tag

Create a new custom tag. Select the template `meteonomiqs - weather tag`.

![Tag Configuration](doc/images/customtag.png "Tag Configuration")

Name your tag (For example, 'UA-Weather') and fill out the following fields.

* API_KEY: Add the API key you have received during registration.

* Custom Dimensions: Create Custom dimensions with the same Weather parameter names (Detailed Weather Status, Grouped Weather Status, Temperature Maximum, Temperature Minimum, Precipitation, Windchill, Sun hours) on your Google analytics property with 'User' scope. Provide the respective Google Analytics custom dimenion's indexes on these fields. You can assign multiple weather parameters to the same custom dimensions. In this case, the values will be separated by a pipe symbol `|`. Weather parameters that are left blank will not be available in the session data later. Make sure you do not reuse the custom dimenion indexes.

![Tag Configuration](doc/images/gav2.png "Tag Configuration")

![Tag Configuration](doc/images/tagv2.png "Tag Configuration")

* Consent Status: Choose the data layer variable 'CMP.WeatherTag' which we created earlier. It checks the user consent for wetter.com. The weather information will ONLY be sent if the return value of this variable is true.

* Push status execution to data layer: Leave this checkbox enabled to get the weather tag execution status in the data layer. The variable 'mtqfired' will be updated every time when the weather tag is requesting for weather information. If the tag is fired successfully, the value is 'yes'. If the value is 'no', the tag needs to be fired again. We use the return value (yes/no) to trigger the weather tag in Step 6.

* Cookie Name Meteonomiqs: _sessmetonmq (this is prefilled)

* Cookie Name Google Analytics: _ga (this is prefilled)

Save the tag. Add this tag ('UA-Weather') as a cleanup tag (Tag sequencing) on your website's generic pageview tag as shown below. The tag sequencing will ensure the custom tag fires immediately after your pageview tag is fired. This is required to associate the weather information with the Google Analytics sessions.

![Tag Configuration](doc/images/sequenc.png "Tag Configuration")

In most cases, Tag sequencing is enough to send the weather information. But sometimes when a visitor has left your page without any interation, empty values may be sent instead of weather information. To avoid that, we add a trigger (Step 6) that will repeatedly fire the weather tag based on the previous execution status.


### Step 6: Configure Trigger

Click on Triggering. Click on + at the top right corner to add a new trigger. Choose the custom event trigger.

![Tag Configuration](doc/images/customeventtrigger.png "Tag Configuration")

Under the trigger firing conditions, check the return value for both the variables 'CMP.GoogleAnalytics' and 'CMP.WeatherTag' we created earlier. The value should be 'true'.

![Tag Configuration](doc/images/triggerv2.png "Tag Configuration")

In addition to that, check the return value of the variable 'dlv - mtqfired'. The value should be 'no'. This means the tag will fire repeatedly until the value is 'yes'. Eventhough the tag is fired multiple times, there is no call to the backend until the value is 'yes'. Save the trigger.



Note: If you don't have CMP in place, the trigger for checking the consent (Step 6) can be ignored. Simply add this custom tag you have created (without any trigger) as a cleanup tag (Tag sequencing) on your website's generic pageview tag as shown here. The tag sequencing will ensure the custom tag fires immediately after your pageview tag is fired.

![Tag Configuration](doc/images/sequence.png "Tag Configuration")


## Usage

Once the tag is configured and published, the custom dimenions of a user session data will contain the configured weather parameters!

You can now analyze how user behaviour is impacted by different weather conditions.

![Tag Configuration](doc/images/samplereport1.png "Tag Configuration")

![Tag Configuration](doc/images/samplereport2.png "Tag Configuration")

Warning: The Weather Tag by Meteonomiqs determines location based on IP address. The location data is then used to check weather conditions. IP address is not saved or processed any further. You should ensure that your website privacy policy complies with the weather tag requirement.

Go ahead an build you own weather based analysis!

## FAQ - Frequently Asked Questions

### How can I check that the tag is integrated correctly?

If the tag is configured correctly, the weather information will be found in the configured custom dimensions in Google Analytics.

To troubleshoot a configuration issue, please check the following points.

#### How can I verfiy that my API key is valid?

You can send a GET request as follows:

```
curl -X GET -H 'x-api-key: <YOUR_API_KEY>' "https://wdx-gtm.meteonomiqs.com/prod/gtm/ip2weather?c=dummy&s=1"
```

If you get a respose with {"message":"Forbidden"}`, then you key is not valid. Please contact our support at info@meteonomiqs.com 


#### How can I check that the correct Google UA ID is used?

You have provided the UA ID during registration. You can check it using you API key:

You can send a GET request as follows:

```
curl -X GET -H 'x-api-key: <YOUR_API_KEY>' "https://wdx-gtm.meteonomiqs.com/prod/gtm/ip2weather?c=dummy&s=1"
```

The response will contain the masked Google UA-ID in the field `"google_uid"`, e.g. "google_uid": "UA-59xxxxxxxx"`

#### How can I see that the tag sends the correct requests to meteonomiqs?

You can use the debug/web view of you browser to look for network traffic to `https://wdx-gtm.meteonomiqs.com/prod/gtm/ip2weather?`.
You should find a POST request like this one:

```
https://wdx-gtm.meteonomiqs.com/prod/gtm/ip2weather?c=GA1.3.371088426.1234567890&s=1
```

The parameter c must contain a valid GA session ID. It must be followed by at least one paramter with a value of the custom dimensions you have configured.

### Why is the tag sometimes not fired on first page visits?

In order to combine the weather information with a Google Analytics session, the tag should be fired after the first Google Analytics pageview tag is fired. In order to check the tag execution status, tick the box "Push status of tag execution to dataLayer" in the tag and create a dataLayer variable "mtqfired". This variable is updated everytime the tag is requesting weather information.

![image](https://user-images.githubusercontent.com/65337449/140804954-f0b5ec8c-2ec1-4707-a0d2-2af4e3a7ab03.png)

If the tag was fired successfully, the value is yes. If the value is no, the tag needs to be fired again.

### How can consent management be integrated?

The tag has an integrated consent trigger. The tag will be fired everytime but cookies and requests will be send only, if the field "Consent Status" is set to true (boolean). Please add a Variable here, that reflects the consent status for this tag.

### What kind of values is expected for weather information boxes in the tag?

![image](https://user-images.githubusercontent.com/65337449/140805760-56879844-5489-4771-b96c-d4c443de09cf.png)

Please fill in the custom dimension index 1 -20 or 1-200 (GA360). 

![image](https://user-images.githubusercontent.com/65337449/140806175-fa33f0ea-70e7-4ea9-93bb-abb6b99bc3ea.png)


### Why are there sessions without weather information?

This can be caused by the following 

* You have visitors that do not interact with you page, see question "Why is the tag sometimes not fired on first page visits?"
* You have exceed you monthly subscription plan, see question "How can I see how much of my monthly plan is already used?"

### How can I see how much of my monthly plan is already used?

This is not yet possible but will be implemented in one of the next releases. If in doubt, please contact info@meteonomiqs.com 
