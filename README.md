# Console commands

The console commands used in the webinar are available in the [console-commands file](https://github.com/grabowskit/transforms-webinar/blob/master/console-commands).  The data sets used are the Kibana sample data, Ecommmerce logs and the airbnb data that is available below, with download and Elasticsearch upload instructions.

# airbnb data set
Airbnb listings ingested into Elasticsearch

Data downloaded from http://insideairbnb.com/get-the-data.html

## Convert price fields to numerics for better import
The CSV files can be imported using [Elasticsearch file upload feature](https://www.elastic.co/blog/importing-csv-and-log-data-into-elasticsearch-with-file-data-visualizer).  The price fields were getting imported as keyword strings, so I needed to remove the dollar sign and commas from the those fields. To remove the fields I used the awk and sed commands listed below from my OSX terminal command line. 

\# cat listings.csv|awk -F'"' -v OFS="\"" '{for(i=1;i<=NF;i++)if($i~/^\$[0-9.,]+$/)gsub(/[$,]/,"",$i)}1' > new-listings.csv

\# sed -i '' 's/\\$//g' new-listings.csv

## When uploading files use advanced settings to change mappnigs and Ingest pipeline

During the listing upload I needed to use the 'override settings' and select "Has header row" for the file upload to work correctly.

To get the location_geo field I created the field by appending Latitude and Longitude fields originally in the Listing.csv via the Ingest Pipeline.  

The default for numeric fields for file upload is to make them long or double types, so I had to covert several of them to keyword.  

I also wanted to have listing_id as a common field across all three indexes (listings, calendar, and reviews), so I needed to change the 'id' field in listings to 'listing_id' during file upload and make sure 'listing_id' was used as a keyword when uploading each of the three files.
