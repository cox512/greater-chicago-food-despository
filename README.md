<h2>ACS5 Scripts</h2>

*Scripts produce JSONs from ACS5 Census Data*

<u>To run:</u>

1. Obtain a Census API key
2. Create api_keys.py in "greater-chicago-food-depository" root directory
3. In api_keys.py write "CENSUS_KEY = " and then paste your census key as a string ("")
4. Run scripts:
   - test_jsons.py: runs all scripts and tests their output
     - Verifies scripts produce valid jsons
   - main.py: produces JSON files

<u>ACS5 Data Tables</u>
 - Detailed: https://api.census.gov/data/2018/acs/acs5?
   - "Most detailed cross-tabulations"
 - Subject: https://api.census.gov/data/2018/acs/acs5/subject?
   - "Overview of estimates available in a particular topic"
 - Data Profile: https://api.census.gov/data/2018/acs/acs5/profile?
   - "Broad social, economic, housing, and demographic information"
 - Comparison Profile: https://api.census.gov/data/2018/acs/acs5/cprofile?
   - "Similar to data profiles but include comparisons with past-year data"

JSON Format:

{'geographic area': {geo_code: {metric1_dict: {}, metric2_dict: {},...},...},...}
<hr>
<h3><u>How to use Main.py</u></h3>

- Add Census data call to script by creating new CensusData instance
  - Required Construction: CensusData(var_dict, table)
  - class CensusData:
    - self.var_dict:
      - dictionary of census table codes and **descriptive** name: *Values name format: topic_property_subproperty*
    - self.table:
      - ACS5 Data Table link
    - self.function_ls: 
      - optional parameter to send modifying function on final_json inside getCensusData function (sequential), example: processRaceData
    - self.geo_ls:
      - default: zip and county, see getCensusData function description
    - getData(self):
      - Calls getCensusData on itself
    - CensusData.class_set:
      - Tracks instances of CensusData


<u>Additional Scripts</u>

**census_response.py**

*helper module for census API*

functions:

- **getCensusResponse**: creates census API query url
  - ACS5 Table URLs:
    - Detailed: https://api.census.gov/data/2018/acs/acs5/variables.html
    - Subject: https://api.census.gov/data/2018/acs/acs5/subject/variables.html
    - Data Profile: https://api.census.gov/data/2018/acs/acs5/profile/variables.html
    - Comparison Profile: https://api.census.gov/data/2018/acs/acs5/cprofile/variables.html
- **getCensusData**: obtains Census data returns as dictionary
  - Uses getCensusResponse for API call
  - By default runs for zip and county, but expandable to other geographies (not tested)
    - By default only keeps zipcodes for Illinois
- **searchTable**: searches variable tables with keyword and function filters
  - ACS5 Variable Table URLs:
    - Detailed: https://api.census.gov/data/2018/acs/acs5/variables.html
    - Subject: https://api.census.gov/data/2018/acs/acs5/subject/variables.html
    - Data Profile: https://api.census.gov/data/2018/acs/acs5/profile/variables.html
    - Comparison Profile: https://api.census.gov/data/2018/acs/acs5/cprofile/variables.html
- county_fips: returns JSON of county_name: fip (reversible)
- **processRaceData**: determines majority race and race percentages, passed into getCensusData function

**file_to_json.py**

*converts CSV and Excel files to the json format*

- County name in table MUST be in the first column or named "County Name"
- Returns list of JSON/Dictionaries to be merged

**dict_merge.py**

*Merges data dictionaries, prevents overwrite*
- Outputs to file and returns list of dictionaries
- Currently does not add missing data keys to geocodes
