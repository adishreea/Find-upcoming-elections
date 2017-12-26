# Find-upcoming-elections

The goal is to: 
  Create the missing /search route
  Ingest the incoming form parameters
  Derive a basic set of OCD-IDs from the address (see below for further explanation)
  Retrieve upcoming elections from the Democracy Works election API using those OCD-IDs
  Display any matching elections to the user
 
Depending on the time of year and whether it's an odd or even-numbered year,  the number of elections in the system can vary wildly. 
Below is an up-to-date list of OCD-IDs that should return an election until the dates they are listed under.
https://github.com/democracyworks/dw-code-exercise-lein-template/wiki/Current-elections

All about OCD-IDs:
OCD-IDs are Open Civic Data division identifiers. A given address can be broken down into several OCD-IDs. 
For example, an address in Birmingham, Alabama would be associated with the following OCD-IDs:
ocd-division/country:us
ocd-division/country:us/state:al
ocd-division/country:us/state:al/county:jefferson
ocd-division/country:us/state:al/place:birmingham
Not all of those are derivable from just an address (without running it through a standardization and augmentation service). 
For example, just having a random address in Birmingham doesn't tell us what county it is in. 
But we can derive a basic set of state and place (i.e. city) OCD-IDs that will be a good starting point for this project. 
This entails...
  lower-casing the state abbreviation and appending it to ocd-division/country:us/state:
  creating a copy of the state OCD-ID
  appending /place: to it
  lower-casing the city value, replacing all spaces with underscores, and appending it to that.
Then supply both OCD-IDs to the election API request, separated by a comma as shown in the curl example below.
Elections can be retrieved from the Democracy Works elections API for a set of district divisions like so:
curl 'https://api.turbovote.org/elections/upcoming?district-divisions=ocd-division/country:us/state:al,ocd-division/country:us/state:al/place:birmingham'
The response will be in the EDN format (commonly used in Clojure) by default, but you can request JSON by setting your request's Accept header to 'application/json' if preferred.

The solution is explained in detail in the report.txt file.
