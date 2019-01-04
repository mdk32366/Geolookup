# Geolookup
Procedures, files, and code for integrating geolookup in Salesforce instances

MDK-1/4/19 - Initial Entry

One of the easiest ways of cleaning data is to use other data as a bonified reference.  For non-profits, the USPS allows the download and use of the United States Zip Code file download.  You can also use an API call to a third party such as Ziptastic.  Two schools of approach on this application: **declarative or trigger**.  I'll outline the trigger, but document the process for declarative, since every trigger you can avoid is just that much less cost in terms of instance maintenance over time (yes, I know performance can be an issue, but all things being equal.)

Procedures:

1.  Create a custom object for holding zip/postal code information.  Below is a table of fields in the USPS download, suggested field types and comments:

USPS Field | Salesforce Field | Type | Comments
-----------|------------------|------|-----------
zip | Zip Code | Number, 9 digits | No hypens, integer
type	| ZipPostalType | Text | Standard, Military, PO Box, Unique
decommissioned | Decommissioned | Checkbox | Still viable?
primary_city	| Primary City  | Text | Renamed the ZipPostalCode Name field (can only be autonumber or text)
acceptable_cities	| Acceptable Cities | Text | Comma separated list of cities within the area
unacceptable_cities	| Unacceptable Cities | Text | Comma separated list of cities not allowed to be referenced
state	| State | Text | Official two letter designator for state or province
county	| County | Text | Name of County | Appears to be a single value, no straddles
timezone	| Timezone | Text | Country/Timezone
area_codes	| Area Codes | Text | Comma separated list of area codes (text for easier import and use)
world_region	| World Region | Text | Two letter official region code (e.g. "NA" = North America)
country	| Country | Text | Official two letter country code
latitude	| Latitude | Geolocation, decimal, 3 places | For mapping and distance calculations
longitude	| Longitude | Geolocation, decimal, 3 places | For mapping and distance calculations
irs_estimated_population_2015 | Population | Number, Integer | Handy and its included in the data

I figure since USPS is maintaining the information in this manner, and others are aggregating it in a way that can be accessed by API, why not build a box big enough to keep it all in?  It isn't that much data in the long run, and once it is in the DB, you can use it for all kinds of reasons.  

One example, with population, which the IRS is drawing from US Census figures, might be demographic penetration - people responding within a zip code / population for zip code.  Lat/Lon is obvious - mapping and distance calculation.  Timezones for auto-planning call time bands for development people, etc.  It's flexible enough to be used internationally, though you may have to rev this to include country codes for dialing.



### References
For creating a Custom Object and then loading the data into an instance:  [US Zip Code Database](https://www.unitedstateszipcodes.org/zip-code-database/)

For accessing postal code information for the US and multiple other countries as an API call: [Ziptastic](https://www.getziptastic.com/).  Ziptastic allows you to make 100 calls to their DB every 24 hours for free.  For international NPs, this may be ideal.

### Future Work
[] Country dialing codes
[] Region codes

