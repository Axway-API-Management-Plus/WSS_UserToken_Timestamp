# Positive-Field-Validation
Sample to create a policy focused on positive validation of attributes dynamically populated on policy call.

## Description

This policy is meant to provide a number of examples to facilitate positive validation of attributes dynamically populated on policy call. Positive field validation is often desirable to negative validation and it removes the diverse possibility of false negatives. By enabling developers at the API level to set the expected data types, element formats, etc, you can enforce new granular levels of security. This policy could be extended with things such as content type validation, schema validation, message size validation, and other security controls meant to ensure messages fall between expected values.

This policy will only pass if mutual authentication is configured in the API Gateway, allowing a client certificate can be presented. This requires the API Gateway HTTPS Listener to Allow or Require Client Certificates. More information on this configuration can be found under the "Configure Mutual Authentication Settings" section of the "Configure HTTP Services" topic on the Axway public documentation site (https://docs.axway.com/bundle/APIGateway_753_PolicyDevGuide_allOS_en_HTML5/page/Content/PolicyDevTopics/general_services.htm). A step by step configuration of mutual authentication can be found in the Client PKI example on the Marketplace: https://marketplace.axway.com/apps/187190#!overview

The basic policy flow is as follows:

- **SSL Filter**: This filter, renamed "Require SSL/TLS with Client Certificate" to more accurately represent the functionality it provides, checks to ensure that a client certificate was presented as part of the SSL handshake when connecting to the HTTPS listener port, and extracts it for further processing. The certificate is stored as an attribute, which by default will be "${certificate}".
- **Extract Certificate Attributes Filter**: This filter, renamed "Extract Attributes From Client Certificate", parses the client certificate to make a large number of certificate attributes available for processing. The created attributes can be easily viewed by right clicking this filter and selecting the "Show All Attributes" option in the policy configuration window.
- **Extract REST Request Attributes**: This filter makes HTTP Headers and Query Parameters available for processing in policy.
- **Certificate Attributes Filter**: This filter, renamed "Cert Attribute Check - Ensure User is US Based", allows you to enforce a digital policy that provides entitlement management via Attribute Based Access Control (ABAC). This is enabled by comparing the certificate attributes that construct the client certificate DN (eg organization [O], location [L]) against accepted values. In this policy, we confirm the country code on the users certificate identifies them as a US based resource.
- **Compare Attribute Filter**: This filter is used twice in the policy, renamed "Ensure user is based one of the accepted office locations" and "Check Params - Positive Validation Scan", compares dynamically populated attribute values against accepted or expected results. The first implementation requires AT LEAST ONE of the accepted values to be true, in this example ensuring a requestor's certificate identifies them as being located out of an acceptable office. The second implementation requires ALL of the URL parameters being submitted to meet expected formats as defined by a regex, as shown below:

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/paramFilter.png "Param Filter")

This filter looks for the following samples, which can be updated to meet the needs of your testing:
- Date: May only contain only 8 numeric digits, focusing on a DDMMYYYY format. This could be extended to limit values on each numeric set, for example only allowing the second pair of numbers for month (MM) to allow 01-12.
- Email: May only contain lowercase letters, followed by an '@' sign, followed by the axwaydemo domain. eg: dwille@axwaydemo.com
- Fname and Lname: Look for a single uppercase letter followed by lower case letters. Numbers, special characters, etc. are not allowed These values must be 1 to 16 characters. eg: Daniel Wille
- ID: This may only contain 6 total characters with the string "ID" follwed by 4 numberic digits. eg: ID1234
- Phone: This may only contain 12 total characters, three digits follwed by a dash follwed by three digits followed by a dash followed by four digits. This supports a phone number in the format: 555-555-5555

This could be used as part of an enrollment service, query service, etc.
- **Set Message and Reflect Message Filters**: These set a basic 200-OK response and reflects it to the policy requestor to provide an expected and easy to digest outcome if the validation passes.

Once updated to fit your environment, this policy can be used in several ways:
- As a standalone service that can be called to validate content.
- It can be called as a policy shortcut as part of other policies created in Policy Studio.
- It can be registered as a REST API via either the API Gateway Rest API Repository  configuration in Policy Studio or by creating a new API in API Manager.
- It can be registered as an Inbound Security Policy for use within API Manager via 'Server Settings --> API Manager --> Inbound Security Policies' in Policy Studio. It may also be used as a Request or Response policy in the same way to validate messages being passed through.

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3


## Install

- Download the PositiveFieldValidation*.xml policy file.
- There are several options to import this file, including Team Development, REST API, and sample scripted options included with the Axway solution. The simplest way however is to open Policy Studio and select 'File --> Import --> Import Configuration Fragment'.

Import Dialog:

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/importFrag.png "Import Fragment")

## Usage

- Send a sample request message to the policy.

Example URL with query params: https://192.168.159.134:8444/posival?date=12122017&fname=Daniel&lname=Wille&id=ID1234&phone=555-555-5555&email=dwille@axwaydemo.com

## Bug and Caveats

N/A


## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"

Contact - Daniel Wille: dwille@axway.com

## License
[Apache License 2.0](/LICENSE)
