# Google Analytics Weather Tag Template

## Features 

The `meteonomiqs - weather tag` allows you to enrich your Google Analytics user sessions 
with the user's local weather conditions!

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

To make sure you have consent from your users to enhance their session data, please update 
your privacy statement (Step 3a) or include wetter.com (meteonomiqs is a brand of wetter.com)
into your CMP as non-IAB vendor.

What we need consent for
* We store a cookie (default name `_sessmeteonmq`, can be change in tag configuration) with value `True` for 30 minutes in order to send the request not more than once per 30 minutes to the meteonomiqs backend
* The request that is sent to the meteonomiqs backend includes the Google session id from the Google Analytics cookie `_ga` (can be configured in the tag)
* From the request the IP address is used to derive the location (latitude, longitude) for determining the weather at that location. 
* The Google session id together with the weather data is sent to Google analyics
* IP address and location are not stored for further processing, but can be logged for troubleshooting. Logs are kept up to 10 days.

**NOTE: The weather tag requires consent for Google Analytics and Google Tag Managager in order to work!**


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
* Privacy Policy of the Data Processor: https://www.meteonomiqs.com/de/datenschutz/
* Storage information: **Cookie (Maximum age of cookie storage: 30 minutes) is set through Weathertag but it is shown as a first Party. No Non-cookie storage**


If you are using a CMP prior to TCF2.0 or some other consent solution, please include the above information in your privacy statement as needed.

### Step 4: Configure Tag

Create a new custom tag. Select the template `meteonomiqs - weather tag`.

![Tag Configuration](doc/images/customtag.png "Tag Configuration")

![Tag Configuration](doc/images/addcustomtag.png "Tag Configuration")

Name your tag and fill out the following fields.

* `API_KEY`: put here the API key you have received during registration
* `Cookie Name Website`: e.g. `_sessmeteonmq`
* `Cookie Name Google Analytics`: usually `_ga`

Next, the custom dimesions fields need to be filled. Create Custom dimensions with the same Weather parameter names (Weather Status, Temperature, Precipitation , Windchill) on your Google analytics property with 'User' scope. Provide the respective custom dimenion's index on these fields.

You can assign multiple weather parameters to the same custom dimensions. In this case the values will be separated by a pipe symbol `|`

Weather parameters that are left blank will not be available in the session data later.

**NOTE: make sure only to use indices of custom dimensions that are not used elsewhere!**

Scroll down to the Advanced settings. Set the tag firing option to `Once per page`.

TODO: change this for checking consent

![Tag Configuration](doc/images/customtagsettings.png "Tag Configuration")

Save the tag.

Finally, fire this custom tag you have created as a cleanup tag (tag sequencing) on your website's pageview tag as shown here. The tag sequencing will ensure the custom tag fires immediately after your pageview tag is fired.

![Tag Configuration](doc/images/sequence.png "Tag Configuration")

## Usage

Once the tag is configured and deployed, the custom dimenions of a user session data will contain the configured the weather parameters!

You can now analyze how user behaviour is impacted by different weather conditions.

![Google Analytics example analysis](doc/images/ga_example.png "Google Analytics example analysis")

Warning: the Weather Tag by Meteonomiqs determines location based on IP address. The location data is then used to check weather conditions. IP address is not saved or processed any further. You should ensure that your website privacy policy complies with the weather tag requirement.

Go ahead an build you own weather based analysis!
