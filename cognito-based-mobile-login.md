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


![testImage](https://github.com/ArtemAlagizov/aws-scenarios/blob/master/images/mobile-app-web-identity-federation.diagram.png)

-------------------------------------------------------------------------------------------------------------------------------
### Used aws services:
* Cognito => to make use of provided identity provider based login
* Amplify => for usage in code to connect with cognito: [docs](https://aws-amplify.github.io/docs/android/authentication)
* IAM => to create roles / permissions for s3 bucket
* S3 => unstructured file storage for the app related objects for users
* CloudFront => alternative to S3, used to deliver static assets to the users
* DynamoDb => noSql db to store the app related info for users

-------------------------------------------------------------------------------------------------------------------------------
### Pricing:
* Cognito => **regular**: First 50,000	monthly active users Free, **free tier**: free with limitations, [cognito pricing](https://aws.amazon.com/cognito/pricing/)
* Amplify => free, only libraries are needed, it seems
* IAM => free
* S3 => **regular**: First 50 TB / Month	$0.023 per GB, **free tier**: free with limitations, [s3 pricing](https://aws.amazon.com/s3/pricing/)
* CloudFront => **regular**: on-demand, data out: First 10TB: $0.085 per GB, **free tier**: free with limitations [cloudFront pricing](https://aws.amazon.com/cloudfront/pricing/)
* DynamoDb => **regular**: complex calculations, **free tier**: free with limitations, [cognito pricing](https://aws.amazon.com/dynamodb/pricing/provisioned/)

-------------------------------------------------------------------------------------------------------------------------------
### Source info: 
* https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc_cognito.html
* https://github.com/aws-amplify/amplify-js/issues/4388
