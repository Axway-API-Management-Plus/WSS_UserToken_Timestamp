# WSS-UserToken-Timestamp
Sample to create a policy that applies WS Security authentication controls by enforcing the inclusion of a WSS Usertoken and a Timestamp on requests.

## Description

This policy provides an example WS Security policy for User Token and Timestamp validation. The associated excersize covers step by step how to use this policy, as well as how to virtualize a web service in the API Gateway, and test it with Soap UI.

The basic policy flow is as follows:

- ***Extract WSS Header Block***: This filter, renamed "Check WSS Header is present and extract", ensures the WS Header block is present and places it into an attribute for use with the other filters.
- ***Extract WSS Username Token***: This filter, renamed "Check WSS Username Token is present and extract", takes the WSS Header block, ensures a username token block is present, and places it into an attribute for use with the other filters.
- ***WS-Security Username Token***: This filter, renamed "Validate WS-Security Username Token", provides configurable authentication and validation of the extracted  WS username token.
- ***Extract WSS Timestamp***: This filter, renamed "Check WSS Timestamp is present and extract", checks the WSS header block for timestamps and extracts them for use with other filters.
- ***Validate Timestamp***: This filter, renamed "Validate Created/Expiry Timestamp", allows you to enforce a format for a timestamp and its validity relative to a selected time zone.

![alt text](https://github.com/Axway-API-Management-Plus/WSS_UserToken_Timestamp/blob/master/example/src/WSSec-Policy.png "WS Sec Policy")

Once updated to fit your environment, this policy can be used in several ways:
- As a standalone service that can be called as part of web service processing.
- It can be called as a policy shortcut as part of other policies created in Policy Studio.
- It can be registered as a REST API via either the API Gateway Rest API Repository  configuration in Policy Studio or by creating a new API in API Manager.
- It can be registered as an Inbound Security Policy for use within API Manager via 'Server Settings --> API Manager --> Inbound Security Policies' in Policy Studio. It may also be used as a Request or Response policy in the same way to validate messages being passed through.

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3


## Install

- Download the WSSec_UserToken_Timestamp*.xml policy file.
- There are several options to import this file, including Team Development, REST API, and sample scripted options included with the Axway solution. The simplest way however is to open Policy Studio and select 'File --> Import --> Import Configuration Fragment'.

Import Dialog:

![alt text](https://github.com/Axway-API-Management-Plus/Positive-Field-Validation/blob/master/example/src/importFrag.png "Import Fragment")

## Usage

- See the example directory for more information on how touse this policy.

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
