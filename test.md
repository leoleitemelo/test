Overview of Maximo Mobile authentication and data flow
======================================================


You can use the Maximo® Mobile app in online and offline scenarios. In an online scenario, the app is connected to Maximo Manage and uses the services and data that are provided. In an offline scenario, the app is not connected to Maximo Manage but continues to operate by using locally stored data.

Login and authentication[](https://www.ibm.com/docs/en/mas-cd/maximo-manage/8.3.0?topic=mobile-overview-maximo-authentication-data-flow#concept_qy1_4j3_k4b__title__2 "Copy to clipboard")
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The first time that a user logs in to the Maximo Mobile app, they must have network connectivity, so they can connect to Maximo Application Suite. To log in to the Maximo Mobile app, the user must enter the credentials that they use in Maximo Application Suite. Using a single sign-on, the user is also authenticated with Maximo Manage, and the mobile apps that are on the Maximo Manage server are set up on the device. As part of the initial setup, a local JSON data store is created on the device. The data store is a repository for data that is downloaded from the Maximo Manage server. The data store is encrypted by using a unique key that is saved in the device's keystore. Each time a user opens the Maximo Mobile app, they are prompted for their device's security credentials so the app can access the local data store.

When a user authenticates to Maximo Application Suite, they are granted a token, which is used to authenticate all requests to the server. The token lasts for 12 hours, after which the user is prompted to authenticate to Maximo Application Suite again. If the user does not authenticate successfully, they work offline until they authenticate to Maximo Application Suite again. A user can authenticate to Maximo Application Suite at any time by clicking the cloud icon at the bottom of the app screen.

For information about Maximo Mobile security, refer to the [Security and user management](https://www.ibm.com/docs/en/maximo-manage/8.2.0?topic=security-user-management "(Opens in a new tab or window)") topic in the Maximo Manage documentation.

Online and offline operations[](https://www.ibm.com/docs/en/mas-cd/maximo-manage/8.3.0?topic=mobile-overview-maximo-authentication-data-flow#concept_qy1_4j3_k4b__title__3 "Copy to clipboard")
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

When mobile users are online, Maximo Mobile apps interact with Maximo Manage and exchange data that is represented in JSON format. In Maximo Manage, requests are processed by an OSLC service provider, and a response is returned.

The data that is retrieved from Maximo Manage is automatically saved to the local data store on the device. The availability of locally stored data makes online data operations more efficient, and users can continue to work when a planned or unexpected disconnection occurs. While users are online, local data is automatically synchronized to maintain consistency with Maximo Manage. Users can also manually synchronize data.

When mobile users are offline, requests are processed on the device by using data that was retrieved during online operations. When connectivity is restored, local data is automatically synchronized.

Some applications, like Assist and Parts Identifier, require the user to be online.

Configuring properties that affect mobile apps
==============================================


In Maximo® Manage, you can set system properties that affect the Maximo Mobile Technician and Inspections apps.

You configure system properties in the System Properties application in Maximo Manage. The following table describes the system properties that you can configure for mobile apps.

Table 1. System properties that affect mobile apps

Property

Description

mxe.mobile.travel.radius

The radius from the user’s GPS location to the work order over which travel time can be tracked. Depending on the user's region, the value is measured in kilometers or miles.

mxe.mobile.travel.prompt

Users can track travel time when the value in mxe.mobile.travel.radius is matched or exceeded. Set to 1 to enable.

mxe.mobile.travel.navigation

In the Maximo Mobile Technician and Inspections apps, when a user clicks Start travel, a map opens to help the user get to the location of the work. Set to 1 to enable.

maximo.mobile.fetch.timeout

The time in milliseconds that the Maximo Mobile app waits for a response from the server and also the minimum time that the app stays offline if a response is not received. After the minimum time that the app stays offline expires, the app tries to connect to the server the next time that the mobile user sends a request to the server.

mxe.app.workorder.InspectionBatchRecord

Set to 1 to create batch records for inspections in the same work order, regardless of the hierarchy of the work order. This property is set to 1 by default. 

mxe.app.workorder.StatusToCreateInspection

Defines the internal work order status where the inspection result is created.

mxe.app.inspection.UpdatePendingResults

Set to 1 to update pending inspection results with the newest active revision.

mxe.mobile.inspection.mobilefeatures

Enables features in Maximo Mobile that are not supported in Conduct an Inspection Work Center.

mxe.mobile.inspection.features.signature

Enables signature input on Inspections forms.

mxe.mobile.inspection.features.multiselect

Enable multiselect input on Inspections forms.

maximo.mobile.usetimer

Specifies whether Start work starts the timer. Set to 0 to change status only and not start the timer.

maximo.mobile.statusforphysicalsignature

The status that requires the user to provide a physical signature.

maximo.mobile.ldap.isForm

This property is used by Maximo Mobile to detect what LDAP authentication method is used.

maximo.mobile.completestatus

The status that the work order changes to **Complete** when work is tapped in Maximo Mobile.

mxe.webclient.resetMobileSCCache

Reset the mobile start center cache. Resetting the cache might cause portlet loading issues.


Configuring default bar code formats
====================================

By default, the bar code reader in the Maximo® Mobile applications tries to read all bar code formats. To reduce the number of formats that the bar code reader tries to read, you must specify one or more default formats that are used in your organization.

Procedure[](https://www.ibm.com/docs/en/mas-cd/maximo-manage/8.3.0?topic=mobile-configuring-default-bar-code-formats#tasktask_phv_m3h_k4b__steps__1 "Copy to clipboard")
------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Open the controller.js file for each app.
2.  Locate the following line in the file:
    
    `'mxe.barcode.readers': ['all_formats']`
    
3.  Change the default value of the property, which is `all formats`, to the default formats that you want to use. If you want to specify multiple formats, use a comma separator, for example:
    
    `'mxe.barcode.readers': [‘code_128_reader’,‘upc_reader’]`
    
    The following bar code formats are supported:
    
    *   code\_128\_reader
    *   ean\_reader
    *   ean\_8\_reader
    *   code\_39\_reader
    *   code\_39\_vin\_reader
    *   codabar\_reader
    *   upc\_reader
    *   upc\_e\_reader
    *   i2of5\_reader
    *   2of5\_reader
    *   code\_93\_reader
    
4.  Save the controller.js file.
5.  Optional: In the Technician and Inspections apps, any button that scans bar codes tries to read all of the default bar code formats that you specified. In the app.xml for either app, you can override the default bar codes formats that an individual button in the app tries to read.
    
    1.  For each app, open the app.xml file.
    2.  Locate the line that contains the button that you want to configure, for example:
        
        `<barcode-button id="barcodebutton1" on-scan="handleBarcodeScan" timeout="30" label="ScanBarcode"/>`
        
    3.  Add the readers property to the line and specify one or more bar code formats, for example:
        
        `<barcode-button id="barcodebutton1" on-scan="handleBarcodeScan" timeout="30" label="ScanBarcode"/> readers=“{[‘ean_reader’]}”/>`
        
    4.  Save the app.xml file.
    
6.  Build the apps and deploy them to the Maximo Manage server.

Setting the URL of the Maximo Application Suite server in the Maximo Mobile app
===============================================================================

Last Updated: 2023-07-31

To use the Maximo® Mobile app with IBM® Maximo Manage, the URL of your Maximo Application Suite server must be set in the app. The URL can be set manually by the mobile user or set centrally for each user by using a Mobile Device Management (MDM) application.

Maximo Mobile supports the standard approach to centrally configuring mobile applications that is defined by the [Appconfig](https://www.ibm.com/links?url=https%3A%2F%2Fwww.appconfig.org%2F "(Opens in a new tab or window)") Community. When you load the Maximo Mobile Android app in an MDM application, the configurable settings are displayed, and you can set their values. For the Maximo Mobile iOS app, some MDM applications can load the configurable settings from an AppConfig.xml file. If your MDM application, can load the configuration setting from an AppConfig.xml file, create an appconfig.xml file and copy the following text to the file:

    <managedAppConfiguration>
    	<version>1</version>
    	<bundleId>com.ibm.iot.maximo.mobile</bundleId>
    	<dict>
    		<string keyName="serverURL">
    		</string>
    		<boolean keyName="allowURLtoBeChanged">
    			<defaultValue>
    				<value>true</value>
    			</defaultValue>
    		</boolean>
    	</dict>
    	<presentation defaultLocale="en-US">
    		<field keyName="serverURL" type="input">
    			<label>
    				<language value="en-US">Server URL</language>
    			</label>
    			<description>
    				<language value="en-US"/>
    			</description>
    		</field>
    		<field keyName="allowURLtoBeChanged" type="checkbox">
    			<label>
    				<language value="en-US">Allow URL to be changed?</language>
    			</label>
    			<description>
    				<language value="en-US"/>
    			</description>
    		</field>
    	</presentation>
    </managedAppConfiguration>

For more information, see the documentation for your MDM application.

If your MDM application does not support the AppConfig.xml file, you can add the following settings and their values in your MDM application:

Table 1. AppConfig settings supported by Maximo Mobile

Setting

Type

Description

serverURL

String

The URL of the Maximo Application Suite server to which users connect.

allowURLtoBeChanged

Boolean

Allow user to change the server URL in the Maximo Mobile app. The default value is true, which allows a user to change the URL in the app if, for example, they want to connect to a different server.

If you are not using an MDM application to manage your devices, you must send the URL of the Maximo Application Suite server to each Maximo Mobile user. Each user must then manually enter the URL of the server in the Maximo Mobile app.

After the URL of the server is set in the Maximo Mobile app, the Maximo Mobile applications, which are deployed on the Maximo Manage server, are set up on the device.

Configuring self-signed certificates for the Maximo Application Suite server on iOS devices
===========================================================================================


If your mobile users use Android devices, a public certificate is required on the Maximo® Application Suite server. Connections from Android devices to Maximo Application Suite servers that have self-signed certificates are not supported. If your mobile users use only iOS devices, a self-signed certificate can be installed on the Maximo Application Suite server. The certificate must be imported to each iOS device.

Procedure[](https://www.ibm.com/docs/en/mas-cd/maximo-manage/8.3.0?topic=immamd-configuring-self-signed-certificates-maximo-application-suite-server-ios-devices#tasktask_t3g_wzq_4pb__steps__1 "Copy to clipboard")
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Download the certificate file.
    
    1.  Access the Maximo Application Suite server page in your browser on a desktop computer. A message indicates that the connection is not private.
    2.  Find and save the certificate of the root certificate authority. On Safari, click to view the certificate, select the root certificate, and drag the file to a folder.
    
2.  Install the certificate profile.
    
    1.  Use Airdrop to send the saved root certificate file to your iOS device.
    2.  Open the Device Settings page.
    3.  In the User details section of the device settings, click Profile Downloaded.
    4.  In the Install Profile page, click Install.
    5.  If you are prompted, enter the iOS device passcode. A certificate warning is displayed.
    6.  Click Install and click Install again if you are prompted.
    7.  On the Profile Installed page, click Done.
    
3.  Trust the certificate.
    
    1.  On the device, go to Settings \> General \> About \> Certificate Trust Settings. The installed root certificate is displayed in the Enable Full Trust for Root Certificates.
    2.  Turn on trust for the certificate that you installed and click Continue. The certificate is trusted and enabled and you can proceed to use the Technician and Inspections apps in the Maximo Mobile app.


    Device requirements for Maximo Mobile
=====================================


Device requirements include the operating system level, biometric authentication, and device sharing requirements.

To run Maximo® Mobile, a device must meet the following system requirements:

*   Android version 10 or 11, iOS version 14.3 , or Windows 10 version 10.0.17763.0 operating system installed.
*   If the device is shared between users, the device sharing must be managed by the device operating system, and each user must have their own workspace and user profile. On iOS devices, to support device sharing, a mobile device management (MDM) solution must be installed on the device.
*   Windows devices must include the WebView2 Runtime.
*   To use Maximo Mobile, Android devices must support Ar Core. Refer to [Ar Core supported devices.](https://www.ibm.com/links?url=https%3A%2F%2Fdeveloper.google.com%2Far%2Fdevices "(Opens in a new tab or window)")