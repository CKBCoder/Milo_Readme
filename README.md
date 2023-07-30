# Building a Conflict Detection System for Milo

## Solution
<br>
    Important Files:-
<ol>
<li>server.py -> Contains API for health_check, get_conflicting_events, get_conflicting_events_different_user
<ol>
<li>get_conflicting_events() -> Finds whether each event is conflicting with any other events</li>
<li>get_conflicting_events_different_user() -> Finds conflicting events for different users</li>
</ol></li>
<li>interval.py -> Contains Interval class</li>
<li>utils.py -> Contains utilities like read_from_csv, write_to_csv</li>
<li>conflicting_events.py -> Contains actual logic for computing conflicting events</li>
<li>test_conflicting_events.py -> Unit test for conflicting events</li>
<li>lambda_function.py -> Entrypoint code for AWS Lambda Function</li>
<li>requirements.txt -> Contains list of dependent libraries.</li>
</ol>

## Design/Architecture
    User Request Flow Diagram
<img width="595" alt="Screenshot 2023-07-30 at 10 26 08 PM" src="https://github.com/CKBCoder/Milo_Readme/assets/1657417/6a540fd6-254b-4674-81b8-196e348f3f59">

## Performance
<p>For better performance we can store it in DB with index for column start_time so that when we query using Order by we can get events in sorted manner. Then our application code which uses two pointer algorithm just takes O(N) time and O(1) space.
</p>

## Scale
<p>For high scale:-
<ol>
<li>We can have a DB where we will store all the events with index on column start_time. When we query using ORDER BY we will get events in sorted order as per start_time.</li>
<li>We can move from serverless to containerization as it will help to scale better and will have less latency and more cost effective.</li>
</ol>
</p>

## Deployment
	
![image](https://github.com/CKBCoder/Milo_Assignment/assets/1657417/be0f127f-4683-4e15-ab8f-d3bc4366f00d)
<p>
    Serverless
	<ul>
        <li>Zip and upload to AWS Lambda Function and Connect it with AWS API Gateway.</li>
        <li>Serverless cost is $0.20 per 1M requests.</li>
	</ul>
    Containerization
<ul>
        <li>Write docker file and containerize the code. Run it using ECS or EKS.</li>
        <li>Cheapest instance cost per hr is $0.0042.</li>
</ul>
 </p>
<p>
Serverless is more cost effective if we have traffic less than 20k requests per hr but when it becomes more than 20k requests per hr using Container becomes more cost effective.
</p>

## Expansion
<ol>
<li>
<b>Prioritizing events by importance</b>
<p>Add a column named priority. We can find all clusters of the conflicting events and among each cluster of the conflicting event we can sort them in descending order of priority and suggest the event with highest priority.
</p>
</li>

<li>
<b>Considering the locations of the events</b>
<p>Add a column name location. Conflicting logic will be <strong>(interval1.end + time(location1, location2) > interval2.start)</strong>
	<br>To find the time between two locations i.e time(location1, location2), we can use Google Maps API.
</p>
</li>

<li>
<b>Alerts system for upcoming events</b>
<p>We can have two types of notifications:-
<ol>
<li>Travel notification -> User will be notified 1hr before so that they have time to travel or when we add location column then we can notify user as per the time taken to reach the destination i.e time(location1, location2)</li>
<li>Reminder notification -> User will be reminded 10 min before the event</li>
</ol></p>
</li>

<li>
<b>Integration with existing calendar systems</b>
<p>We can integrate with Google/Apple Calendar to use their functionalities relevant for our needs.
</p>
</li>

<li>
<b>Training Milo to learn user behavior and predict preference</b>
<p>When there is a conflict, feed the user preference to the AI model which learns and recommends the preference for each events in case of conflict.
</p>
</li>
</ol>
