# airbnb
Airbnb listings ingested into Elasticsearch

Data downloaded from http://insideairbnb.com/get-the-data.html

## Clean the data
The CSV files can easily be imported using Elasticsearch file upload feature.  The price fields were getting imported as keyword strings, so I needed to remove the dollar sign and commas from the those fields. To remove the fields I used the following awk and sed commands.. Since I am running on OSX, I have listed the Mac terminal commands.

\# cat listings.csv|awk -F'"' -v OFS="\"" '{for(i=1;i<=NF;i++)if($i~/^\$[0-9.,]+$/)gsub(/[$,]/,"",$i)}1' > new-listings.csv

\# sed -i '' 's/\\$//g' new-listings.csv

