# SSH-Splunk-Log-analysis-


Title: Splunk SSH Log analysis 
Analyst: Savon Masters 
Date: January 31st 2026

					Summary
On April 24th, 2025 6:25 am I received an alert about a potential Brute force attack on the SSH system that had taken place. I investigated to see the legitimacy of the alert.

					Investigation 

Title: Splunk SSH Log analysis 
Analyst: Savon Masters 
Date: January 31st 2026

					Summary
On April 24th, 2025 6:25 am I received an alert about a potential Brute force attack on the SSH system that had taken place. I investigated to see the legitimacy of the alert.

					Investigation 




First, I wanted to see how many logs there were in the file and how long the event took place, which I did by looking at the timeline.


Second, I wanted to see the different type of events in the log and how many of each took place, I used the “Stats count by event_type” to do this. 


Thirdly, I wanted to see the number of failed SSH logins and where they were originating and the amount of times they tried to access the system, I added the “Stats count by id.orig_h | sort - count” to figure this out.  This could be a potential Brute force or Password spraying attack for the reason that there was so many login failures from the same accounts.



Fourthly, I wanted to see the number of Multiple Failed Authentication Attempts from the source IP address and the destination IP address it tried to enter into and the number of times it occurred, I searched “Event_type Multiple Failed Authentication Attempts | stats count by id.org_h, id.resp_h | sort - count.”. This is a complete indicator of a Brute force attack for the amount of failed authentications attempts in the short span of time.



Fifthly, I wanted to see the number of Successful SSH Logins and the individuals accounts it accepted, to accomplish this I used “Event_type Successful SSH logins | stats count by id.org_h, id.resp_h | sort - count.”.



I wanted to compare the Successful SSH Logins with the Multiple Failed Authentication Attempts to see if the account was compromised. It was able to happen on the account “10.0.0.28” since it was Brute forced and was able to logon. 
 

Sixly, I searched for the Connection without Authentication as a caution because Connections without Authentications often can be used for port scanning. I queried “Event_type Connections without Authentication | stats count by id.org_h, id.org_p, id.resp_h, id.resp_p | sort - count.” and brought in the information. 

					Conclusion 
I finalized that a Brute force attack took place April 24th, 2025 6:20:09:508 am - 6:20:09:524 am. I could find that nothing else happened on that day in the system. The account “10.0.0.24” and others were infected. 

					IOCs
There were over 808 Failed SSH Logins and Multiple Failed Authentication Attempts that were recorded in this event.
This was a Brute force attack based on the fact to have this excessive login failures in the span of 1 second.
One of the accounts “10.0.0.24” was compromised shown by it appeared on the Successful SSH Logins and Multiple Failed Authentication Attempts event types.
A big amount of Connections without Authentication which can be used for port scanning.

					Recommendations 
Create an alert for Failed SSH Logins and Multiple Failed Authentication Attempts to be aware of the attack quicker.
Create a rule to block IP addresses that try Multiple Failed Authentication Attempts in a shorten time window. 
Monitor Connections without Authentications for suspicious behaviour. 

I went ahead and made a dashboard of the amount of times Multiple Failed Authentication Attempts and the Successful SSH Logins to gather if the highest failed authentications lead to any successful SSH logins.


I also created an alert to see the Multiple Failed Authentication Attempts happens and the source and destination IP address as part of the alert to be in front of the attack before it transpires. 


					Things I Learned 
Better use of the Splunk platform through alerts, dashboards, and SQL.
How to watch Brute force, password spraying, and port scanning in normal logs.
The right way to analyze SSH logs. 


