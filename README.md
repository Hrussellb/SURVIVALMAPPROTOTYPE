This project are base on where could you go when the world are breaking down so where are you going when you

are in New York city. You don't want to hunt random animal or hunt down animal that are rare also on the

protected lists. So where do you go? By using this type of data set to find out all the location and population

of each types of animal as well as rural areas. Getting an get an ideas some location you could go base on the zipcodes.

All datasets are import into KNIME also using KNIME for preprocessing dataset and calculate danger animal score.


Datasets that are use:

Biodiversity\_by\_County\_-\_Distribution\_of\_Animals\_\_Plants\_and\_Natural\_Communities

https://data.ny.gov/stories/s/Biodiversity-by-County-Distribution-of-Animals-Pla/m65y-hnud\

DECENNIALDHC2020

https://data.census.gov/table?q=P2&d=DEC+Demographic+and+Housing+Characteristics

New\_York\_State\_ZIP\_Codes-County\_FIPS\_Cross-Reference\_20250413

https://data.ny.gov/Government-Finance/New-York-State-ZIP-Codes-County-FIPS-Cross-Referen/juva-r6g2/about\_data


How to import and use Node in KNIME


Loading files into CVS reader:

Do three CVS reader on for three of the files. First CVS reader node import New\_York\_State\_ZIP\_Codes-County\_FIPS\_Cross-Reference\_20250413,

second CVS reader import DECENNIALDHC2020 ,and last CVS import Biodiversity\_by\_County\_-\_Distribution\_of\_Animals\_\_Plants\_and\_Natural\_Communities.


After loading first CVS reader node:

1. Use Missing value node to find any missing row or column.
2. Use Number to String node to convert Zipcode into string.
3. Use Joiner node to join Matching rows base on Zipcode with second CVS reader but after you went through all the node filter for second CVS reader.

After loading second CVS reader node:

Use Missing value node to find any missing row or column.

1. Use Column Renamer node to rename (NAME, P2\_001N, P2\_002N, P2\_003N) into (Zipcode, Population total, Urban total, Rural total)
2. Use String Manipulation node to remove ZCTTA from zip codes. Expression should look like this : replace($Zip code$, "ZCTA5 ", "")
3. Use String to Number node on Population total, Urban total, and Rural.
4. Use Missing value node to find any missing row or column.
5. Same Joiner node as #3 on first CVS reader node.


After loading third CVS reader node:

Missing Value node to find any missing row or column.


After all the CVS are corrected then use Joiner node to join all three file after filtering as (Connty Name, County) with matching rows.

In order to get the NY score and Federal Listing Status score two Rule Engine Node are use.

First Rule Engine Node setting:

$NY Listing Status$ = "Game with open season" => 5

$NY Listing Status$ = "special concern" => 3

$NY Listing Status$ = "threatened" => 2

$NY Listing Status$ = "Game with no open season" => 5

$NY Listing Status$ = "Endangered" => 1

$NY Listing Status$ = "Protected Bird" => 1

$NY Listing Status$ = "not listed" => 0

$NY Listing Status$ = "not applicable" => 0

$NY Listing Status$ = "Protected Bird - Game with open season" => 6

$NY Listing Status$ = "Rare" => 4

TRUE => 0


Second Rule Engine Node:

$Federal Listing Status$ = "Game with open season" => 5

$Federal Listing Status$ = "special concern" => 3

$Federal Listing Status$ = "threatened" => 2

$Federal Listing Status$ = "Game with no open season" => 5

$Federal Listing Status$ = "Endangered" => 1

$Federal Listing Status$ = "Protected Bird" => 1

$Federal Listing Status$ = "not listed" => 0

$Federal Listing Status$ = "not applicable" => 0

$Federal Listing Status$ = "Protected Bird - Game with open season" => 6

$Federal Listing Status$ = "Rare" => 4

TRUE => 0

Filter out the Data after passing throuh two Rule Engine Nodes by using Column Filter node by includes:

County Name, Population total, Urban total, Rural total, Zipcodes fixed, Category, Taxonomic Group, Taxonomic Subgroup, Scientific Name,

Comman Name, NY\_status\_score, Federa\_status\_score

The datasets are now complete and could be use to do K-mean and Desision Tree predictor to predict NY\_status\_score. We will do two K-mean and one

Descision Tree predictor. First K-means to show Histogram node on Federal\_status score, and NY\_status\_score. Second K-mean to show population total base on county name.

Second K-mean also show Box plot for max and min for population, Urban, Rural total.


First Decision Tree predictor:

1. Connect the Column Filter to Number to String node and includes (NY\_status\_score, Federal\_status\_score) inside the node setting
2. Connect Number to String node to Color Manager node and choice Taxonomic Group using Set 1.
3. Connect Color Manager node to Partitioning node with Realtive[%] at 80. Choice to Draw randomly.
4. Connect Partitioning node to Decision Tree Learner node and in Clas column choice (NY\_status\_score) leaving other setting at default.
5. Connect Desision Tree Learner node and Partioning node to Decision Tree Predictor node then click Apply.
6. Connect Descition Tree Predictor to Scorer node and First Column select(Federal\_status\_score) then Second Column(Prediction(NY\_status\_score)


Second K-mean:

1. Use the same Column Filter as Decision Three and connect to K-Means node using setting such as inlcude(NY\_status\_score, Federal\_status\_score), exclude all

others. Use number of cluster is 8.

2. Connect K-Means node to Historgram node and Demension set to (Federal\_status score), No.bins = 20, Binning Type( Equal width) while (Ignore special values), other left at default
3. Conect K-Means node to a new Histogram node and Demension set to (NY\_status\_score), No.bins = 20, Binning Type( Equal width) while (Ignore special values), other left at default


Third K-mean:

1. Use the same Column Filter as Decistion Three and connect to K-Means node using setting such as include(Population total, Urban total, Rural total) and exclude all other.

Number of clusters are 62.

2. Connect K-Means node to Scatter plot with setting as Horization dimension(County Name), Vertical dimension(Poluation total), Max rows(10000000) and leaving other at default.
3. COnnect K-Means node to a new Box Plot with default setting to display



