# pinpoint-incremental-analytics

Background 

Tracking users’ activity is vital for understanding your customers and identifying opportunities. In a data driven world, having actionable data is fundamental but being able to feed these data into your marketing platforms and use them in business logic often comes as a challenge. It is not uncommon for companies in such situations to either resort in manual processes resulting in missed opportunities or try to build something in-house, which won’t be scalable. Currently Pinpoint cannot aggregate events neither create segments based on an event’s count for a specific period.
Solution 

This solution expands Pinpoint’s existing segmentation and event based journey capabilities, allowing the creation of business rules on events’ count and / or summed metric value per user with the possibility to add date filters. When a rule is met, a user attribute with the event name will be updated to “Ready” or an event with the name trk_event will be fired depending the needs of the Pinpoint user. Business rules will be added through HoneyCode and will require no coding experience. 

Use case(s) 
User segmentation based on:
- Aggregated user activity
- Aggregated user activity throughout a specified period

Considerations
1)	You will need to install Amplify SDK for sending events to Pinpoint and Cognito for user management
2)	Only custom metrics are being processed at the moment
3)	HoneyCode is available only in Oregon (us-west-2) region at the moment but the solution will work in any region
4)	When you insert a rule that is using the date filters in Honeycode you will need to wait till it performs its first scan based on the CloudWatch Event Rule scheduling settings. By default, the project sets the CloudWatch Event Rule interval to 60 minutes
5)	You can only define date periods with start and end date. The date format needs to be strictly YYYY-MM-DD
6)	The Lambda used for time series querying is using Pandas library as a Layer (ARN of the layer is public)
7)	All metric calculations and user attribute changes are made on a user level and NOT on an endpoint level
8)	Build the HoneyCode table as per this guide otherwise the solution won’t work as expected. There is a dependency between the Lambda code and the order of the HoneyCode columns
9)	Make sure that the values you are inserting in HoneyCode match exactly the values in Pinpoint (case, spaces etc.)
10)	When removing an aggregate business rule from HoneyCode then all related records from the DynamoDB aggregate table will be deleted (assessment every 60 minutes via Time Series Lambda CloudWatch Event Rule)
11)	Aggregate table records only events that match HoneyCode rules, whereas time series table records all events
