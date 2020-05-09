# Module: MMM-withings
The `MMM-withings` module is an extension for [MagicMirror](https://github.com/MichMich/MagicMirror). It provides a way to display data from the Withings Health API in HTML5 plots using Chart.js

![screenshot](screenshot.png)

## Enable Authorization automatically
A mechanism is now included to enable authorization without manually generating an API key and developer app.

1. Have a withings account
2. Populate config.js with the parameter to attempt authorization:
````javascript
{
    module: "MMM-withings",
    config: {
        attemptAuthorization: true
    }
},
````
3. Launch magic mirror with withings module included.
4. After some time, the default browser will open on the default display linking to withings.com
5. Log in with withings account, and authorize Personal Mirror Project
6. After page returns with OK, you can close the browser. Data should start loading into the withings module.

## Setting Up API Key and User Account (Old manual method)
The following steps are necessary to use this module:
1. Have a withings account:
2. Navigate to [here](https://account.withings.com/partner/add_oauth2) to create an application (can be a fake application)
    1. Application Name: Can be anything
    2. Description: Can be anything
    3. Contact Email: Your Email
    4. Callback URL: Your HTTPS website/callback URL or https://example.com
    5. Application Website: Your Website or https://example.com
    6. Company: Your company or whatever you want
    7. Logo: An image file that meets requirements. 'logo.jpg' In this repo works.
3. Populate config.js with the clientId, consumerSecret, and redirectUri (the Callback URL):
````javascript
{
    module: "MMM-withings",
    config: {
        clientId: 'deadbeefdeadbeef',
        clientSecret: 'deadbeefdeadbeef',
        redirectUri: 'https://example.com',
    }
},
````
4. Once you have a client ID and consumer secret created, create and navigate to the following website
````url
https://account.withings.com/oauth2_user/authorize2?response_type=code&redirect_uri=https://example.com&scope=user.info,user.metrics,user.activity&state=1&client_id=<your_client_id>
````
5. Login with your account credentials
6. Allow this app
7. You will be redirected to an example.com url with a code in the url
E.g.
````url
https://example.com/?state=1&code=deadbeefcafebabe12345789
````
8. Copy the value for code into tokens.json in the following format
````json
{
    "code":"deadbeefcafebabe12345789"
}
````
9. Start Magic Mirror within 30 seconds. On first run, an access/refresh token will be generated. If access code is unable to be generated, an attempt will be made in 30 seconds. Simply repeat steps 4-8, and an access token should be able to be generated.

## Using the module

To use this module, add it to the modules array in the `config/config.js` file:
````javascript
modules: [
    {
        module: "MMM-withings",
        position: "bottom_bar",	// This can be any of the regions.
        config: {
            // See 'Configuration options' for more information.
            units: 'imperial',
            measurements: ['weight', 'fatRatio']
        }
    }
]
````

## Configuration options

The following properties can be configured:

| Option | Description
| ------ | -----------
| `units` | Units to display<br><br> **Default value:** `config.units`
| `userName` | Name of user<br><br> **Default value:** `MagicMirror`
| `attemptAuthorization` | Whether to attempt getting an authorization code using default app<br><br> **Default value:** `false`
| `clientId` | Client Id from step 3<br><br> **Default value:** ``
| `clientSecret` | Consumer Secret from step 3<br><br> **Default value:** ``
| `redirectUri` | Callback URL from step 3<br><br> **Default value:** ``
| `initialLoadDelay` | Delay for first check<br><br> **Default value:** `0`
| `updateInterval` | Update interval in milliseconds<br><br> **Default value:** `5 Minutes`
| `daysOfHistory` | Days of data history to fetch<br><br> **Default value:** `14`
| `measurements` | Array of measurements to check<br>**Possible values:** `weight`, `height`, `fatFreeMass`, `fatRatio`, `fatMassWeight`, `diastolicBloodPressure`, `systolicBloodPressure`, `heartPulse`, `temperature`, `sp02`, `bodyTemperature`, `skinTemperature`, `muscleMass`, `hydration`, `boneMass`, `pulseWaveVelocity`<br>**Example:** `['weight', 'fatRatio']`<br>**Default value:** `['weight']`
