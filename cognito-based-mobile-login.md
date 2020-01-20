### Scenario:
* Enable users of a mobile app to log into the app using Identity Providers as Google and Facebook
* Store relevant user information in s3 bucket / DynamoDb

-------------------------------------------------------------------------------------------------------------------------------
### Approach:
* Register for a developer ID at google, facebook, amazon
* Configure the app with each of these providers (at cognito interface)
* In aws account that contains s3 bucket and dynamoDB table use cognito to create IAM roles that precisely define permissions for the app
* In the app code call sign in interface for the IDP configured previously 
* Use aws amplify service
* Once user logs in the app gets OAuth access token
* The app can trade the token for a set of temp security credentials that can be used to access aws services:
  * Access key id
  * Secret access key
  * Session token




-------------------------------------------------------------------------------------------------------------------------------
### Used aws services:
* Cognito => to make use of provided identity provider based login
* Amplify => fof usage in code to connect with cognito: [docs](https://aws-amplify.github.io/docs/android/authentication)
* IAM => to create roles / permissions for s3 bucket
* S3 => storage for the app related objects for users
* DynamoDb => noSql db to store the app related info for users

-------------------------------------------------------------------------------------------------------------------------------
### Source info: 
* https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc_cognito.html
* https://github.com/aws-amplify/amplify-js/issues/4388
