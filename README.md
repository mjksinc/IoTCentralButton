# Connect teXXmoButton to IoT Central

This document contains all the instructions for connecting your teXXmo IoT Button to Azure IoT Central and triggering an action based on the number of clicks.

## Pre-requisites
- 1 x teXXmo Button
- A Microsoft Account or Azure Subscription
- A computer or tablet with a web browser
- A wifi connection requiring only an SSID and Password to connect

# Acknowledgements
+ [IoT Central Documentation](https://docs.microsoft.com/en-us/azure/iot-central/)
+ [teXXmo Button Page](https://catalog.azureiotsolutions.com/details?title=teXXmo-IoT-Button&source=home-page)
+ [Device Provisioning Service GitHub](https://github.com/Azure/dps-keygen)

# Overview
Something as simple as a button can have powerful actions when connected to Azure. In this example, you'll be connecting a physcial teXXmo button to Microsoft's new software-as-a-service application, IoT Central. The button will trigger an alert if pressed 3 times within a short time frame. You'll learn how to deploy your own IoT Central application, create device templates, and use the Device Provisioning Service to connect the teXXmo button to your application.

## Setting Up IoT Central
1. Head on over to the the [IoT Central Homepage](https://azure.microsoft.com/en-au/services/iot-central/) to learn more or select [Getting Started](https://apps.azureiotcentral.com/) to begin creating your application.

![IoT Central Main Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.1.png)

2. You'll be prompted to login in using your Microsoft account. Enter your credentials or create a new account using the links provided. 

![Microsoft Account login page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.2.png)

3. Follow the prompts until you arrive at the **Application Manager** page. Select the *New Application* tile to get started.

![IoT Central - New Application](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.3.png)

4. Now you'll configure the properties of your new IoT Central application:
   - Select the *trial* payment plan (or the pay-as-you-go option if you want to keep the applciation longer than 7 days)
   - Select *Custom Application* as the template
   - Enter a suitable *Application Name* and use a unique *URL*
   - Press *Create* when done

![IoT Central - Configuration Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.4.png)

5. That's it! You should now the see the main dashboard of your new IoT Central Application.

![IoT Central - Application Home Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.5.png)

## Creating a Device Template
1. Select *Create Device Templates* from the tile on the main dashboard to create a new device template

![IoT Central - Application Builder, Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.5.1.png)

2. Enter a unique name for you Device Template and select *Create* when complete.

![IoT Central - Create Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.1.png)

3. This will generate a Simulated Device which you can use to create graphs, build dashboards, and get your application together before needing to connect a physcial device. Since we're using a physical device, we'll delete the simulated device. Select *Delete* to remove

![IoT Central - Delete Simulated](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.2.png)

## Creating and configuring a new physical Device
1. You should see the *Device Explorer* window on-screen. If not, select the *Explorer* icon from the right-side menu

![IoT Central - Delete Simulated](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.4.1.png)

2. Select the Device Template you created, then click *+ > Real* to create a new real device

![IoT Central - Delete Simulated](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.4.2.png)

3. Enter the following details to create the device:
   - A unique *Device ID*. This will be used for the Device Provisioning Service. A MAC Address or Serial Number are suitable IDs
   - A *Device Name*. This could be the same as the Device ID, or something completely different like location the button will be placed
   - Select *Create* when completed

![IoT Central - Create Device Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.4.png)

4. You should now be on the **Device Configuration** page for the button you just created. If not, select *Device Explorer* in the right-side menu and navigate to your new device.

![IoT Central - View Device Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.5.png)

5. You'll now configure the device's measurements. Specifically, the device will transmit a click *event*. Make sure you're in the *measurements* tab, then click *Edit Template*

![IoT Central - Edit Measurement](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.5.1.png)

6. Select *+ New Measurement > Event* (you may need to scroll depending on the zoom level of your browser)

![IoT Central - Create Event](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.6.png)

7. Now you'll configure the event parameters:
   - Enter a *Display Name* for how the event will be labelled in your application
   - Enter a *Field Name* for the measurement. __Note:__ This field name will need to be remembered for when you configure the button settings in future steps
   - Set the *Default Severity* to any level for this use case
   - Select *Save* when you're finished

![IoT Central - Event Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.7.png)

8. A *Rule* needs to be created so your IoT Central application knows when to trigger an action. Select the *Rules* tab and click *Edit Template*.

![IoT Central - Create Rule](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.8.png)

9. To add a new rule, select *+ New Rule* then choose *Event*. This should match the Measurement type selected in the previous steps

![IoT Central - Edit Rule](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.9.png)

10. You'll now create the rule that IoT Central will monitor. In this case, at least 2 button presses within 5 minutes. Configure the rule with the following properties:
    - *Name* should be descriptive of the rule's action for easy understanding
    - Select *Enable* for both the template and device options
    - Click the *+* symbol next to *Conditions*
      - Click the dropdown box under *Measurement* and choose the event you created earlier
      - For *Aggregation*, select *Count*
      - For *Operator*, select *is greater than or equal to*
      - Enter the *Threshold* as *2*
    - Select the *time duration* under the *Aggregation time window* as 5 minutes
    - Click *Save* to confirm properties

![IoT Central - Rule Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.10.png)

11. Now you've created the **Rule**, let's create the **Action** IoT Central should take if the Rule is triggered
    - Select the Rule you just created
    - Scroll until you see *Actions* in the window and click the adjacent *+* symbol.
    - Select the email tile
    
![IoT Central - Rule Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.11.png)
    
12. Now you'll need to configure the email that's sent when the rule is triggered:
      - Enter a *Display Name*
      - Recipient addresses in the *To* field. **Note:** They have to have logged into your IoT Central applcaition previously to receive the email
      - A message to be included in the email under *Notes*
    - Click *Save* when complete
    
![IoT Central - Rule Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.12.png)
    
13. Congratualations! You've succssfully created a real device in IoT Central and configured a rule to be actioned based on events from the button

## Generating a SAS Token using Device Provisioning Service
1. On the same page as your Measurement and Rule creation, select *Connect* at the top right of screen This will generate credentials for you to create a connection string through the **Azure Device Provisioning Service (DPS)**.

![IoT Central - Connect](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/image3.2.png)

2. Save the following credentials for later use:
   - Under **Device Connection**, copy the *Scope ID* and *Device ID*
   - Under **Credentials**, select *Shared Access Signature*, then copy the *Primary ID*
   - Select *Close* when complete
   
 ![IoT Central - Credentials](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/image3.3.png)
   
3. A DPS client will now need to be installed to generate the Connection String for the real device. Install the **DPS Key Generator** (ds-keygen) through your Command Line Interface by following the instructions on the [DPS-KeyGen GitHub Repo](https://github.com/Azure/dps-keygen)

4. Now, install the platform-specific dps_cstr tool as instructed on the [dps_cstr tool GitHub Repo](https://github.com/Azure/dps-keygen/tree/master/bin)

5. Execute the following command, replacing the arguments with the credentials you saved in Step 2 above.

```dps_cstr <Scope ID> <Device ID> <Primary ID>```

7. Copy the entire output of this command and save your later use. Your saved output should look similar to the string below:

```HostName=saas-iothub-51a5f13a-dfd7-42a1-862c-d756bb08a236.azure-devices.net;DeviceId=sw9u;SharedAccessKey=UJQKvC4sfHW0ux7IfhN2qJEMbFztcbD3xEDadHHVIFk=```

6. Now you've generated your connection string, it's time to connect your teXXmo button!

# TeXXmo Button
**Note:** these steps are also available under "*Getting Started*" [on the teXXmo page](https://catalog.azureiotsolutions.com/details?title=teXXmo-IoT-Button&source=home-page) of the Azure Device Catalog

## Configuring your button
1. Put the teXXmo button into Access Point (AP) mode by pressing and holding the button for 6 seconds. The LED will change to a yellow flashing strobe, then to a pulsing red.
    **Note:** if the button begins to rapidly flash green, wait until it stops, then try again.
    
2. Open the network connection settings on your desktop and select the wifi network beginning with *ESP_XX:XX:XX* (The numbers will match the MAC address of your button). The light will continue to pulse red while in AP mode. 
    **Note:** this will disconnect you from the internet
    
3. Open a web browser and go to *192.168.4.1*. You should arrive at the homepage for your button.

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/image4.1.png)

4. Select the *IoT Hub Configuration* tab at the top left of the screen. This is where we'll break down the connection string generated from DPS section:
   - Copy the *Azure IoT Hub* from the *Hostname*
   - Copy the *IoT device name* from the *Device ID*
   - Copy the *IoT device secret* from the *Shared Access Key*
  
  ![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/image4.2.1.png)
  
  ![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image4.2.2.png)
  
5. Select the *WIFI* tab at the top of the screen and enter the SSID and Password for your wifi network to connect the button to the internet. Select *Save* when complete

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image4.3.png)

6. Next, click the *User JSON* tab. This is where you can write the JSON message the device will deliver when clicked. Enter the text ```{"click": "true"}``` and click *Save* when complete
**Note:** the key (in this case, "click") should match the Field Name you entered into IoT Central previously

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image4.4.png)

7. Now select the *Shutdown* tab and click the *Shutdown* button to exit the homepage and save your configuration to the button

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image4.5.png)

8. Reconnect to your computer to the original wifi network and return to your IoT Central Application (either through the direct url, or going to [apps.azureiotcentral.com](apps.azureiotcentral.com) and logging in with your credentials)

## Sending Telemetry and Visualising in IoT Central
1. Now the physical button has been connected, return to the device page in your IoT Central Application by clicking *Device Explorer*, then the device template and device created previously

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image5.1.png)

2. Click the *Measurements* tab to display the previously-created Event measurment.

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image5.2.png)

3. Push and hold the physical teXXmo button for ~1 second then release. You should see the green LED frantically flash, then emit a single green pulse to signify successful data transmission

4. After a few seconds, the event should plot onto the **Measurements** graph. Congratulations - you sent your first message to your IoT Central Application!

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image5.3.png)

5. Press the buttton several more times to trigger the rule you created. This should trigger the email action you created earlier
**Note:** IoT Central checks aggregation in tumbling windows starting on the hour.

![teXXmo welcome page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image5.4.png)

6. Now you've set up your IoT Central Application, explore some of other features such as the [Creating a Webhook Alert](https://docs.microsoft.com/en-au/azure/iot-central/howto-create-webhooks) or [Exporting Data](https://docs.microsoft.com/en-au/azure/iot-central/howto-export-data)
