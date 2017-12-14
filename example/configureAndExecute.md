# WSS-UserToken-Timestamp
Sample to create a policy that applies WS Security authentication controls by enforcing the inclusion of a WSS Usertoken and a Timestamp on requests.

## Prerequisites

To follow this example you will need the following:

- This example uses SoapUI for testing. You may use your own tools, but this example only gives client setup instructions for this tool.
- The policy export file that is part of this repository.
- Basic working knowledge of the Axway API Gateway software and Policy Studio.


## Step By Step

1. Navigate to 'File --> Import --> Import Customization Fragment'. Select the policy fragment supplied in this repository. Resolve any dependency issues and complete import.

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/importFrag.png "Import Fragment")

2. Modify the policy to meet your environmental configuration.

3. For this example we will be using a public web service called 'Global Weather'. Create a new SoapUI project, and specify the following URL for the WSDL: http://www.webservicex.net/globalweather.asmx?WSDL. Import all operations and create sample requests as prompted.

4. Choose the GlobalWeatherSoap12 GetCitiesByCountry operation. Modify the request with a country name as shown and send a sample request through to the prepopulated endpoint by pressing the green button in the top left of the window.

```
<web:CountryName>United States</web:CountryName>
```

Confirm you are able to get a response from the remote service.

5. In Policy Studio, navigate to "APIs --> Web Service Repository --> Web Services". Right click on this folder and choose to "Register Web Service". Follow the prompts, select both sets of operations, and default configuration. The result should be a new web service that shows up as "Global Weather" under the Web Services repository, as well as a new "GlobalWeather" policy under "Generated Policies --> Web Services.GlobalWeather". Deploy.

6. Navigate to the new GlobalWeather policy. Confirm that it includes the default relative path '/globalweather.asmx'. Return to SoapUI and create a new request for the GlobalWeatherSoap12 GetCitiesByCountry operation and update the request with the country name. Modify the URL to point at you API Gateway. Assuming you are using the default HTTP services, ports, and the default endpoint, your new URL might look like this: http://apihost:8080/globalweather.asmx. Run through a request against the end API Gateway endpoint. This should provide a response similar to the direct web service test in step four.

7. Right click on the top level of your SoapUI project and select "Show Project View --> WS Security Configurations --> Outgoing WS Security". Here you will be creating a new WS Security Outgoing Profile with a username and timestamp set up. To keep this testing simple, any test username and password will do as we do not authenticate it against a repository in the sample policy. Based on my testing, in order to use the updated profile you now need to save the project, close SoapUI, restart it and navigate back to the project. Open the request, go to the auth tab, create basic auth policy. Add sample user/pass and on outgoing WSS choose your created option.

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/attributeCapture.png "Attribute Capture")

8. Return to Policy Studio. Double click the Service Handler Filter and choose "Message Intercept Points --> Request from Client --> and click the three dots next to option A) Execute Before Operation". Choose the  WSS_UserToken_Timestamp policy you imported earlier. Deploy the changes.

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/attributeCapture.png "Attribute Capture")

9. With the new SoapUI WSS Outgoing profile active, send the request against the API Gateway endpoint and confirm it passes.

10. Log into your API Gateway Management web console (eg https://hostmane:8090), and access the Traffic Monitor. Review the message execution.


## Next Steps

From here you can play around with enabling nonces, using digests, enabling repos, and more. You could also register the WSDL in API Manager, configure the WS Sec policy as an Inbound Security Policy for API Manager, and test there as well.

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"

Contact - Daniel Wille: dwille@axway.com


## License
[Apache License 2.0](/LICENSE)
