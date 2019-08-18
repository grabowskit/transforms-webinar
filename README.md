# airbnb
Airbnb listings ingested into Elasticsearch

Data downloaded from http://insideairbnb.com/get-the-data.html

## Clean the data
I needed to clean the data by removing the dollar sign and commas from the price fields.  to do this I use the following ACK and SED commands.  Since I am running on OSX, I have listed the Mac terminal commands.  just replace filenames

cat listings.csv|awk -F'"' -v OFS="\"" '{for(i=1;i<=NF;i++)if($i~/^\$[0-9.,]+$/)gsub(/[$,]/,"",$i)}1' > new-listings.csv

sed -i '' 's/\\$//g' new-listings.csv

