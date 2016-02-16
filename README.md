# pubfind
Find missing publications from Scopus.

# Installation

   $ pip install pubfind

# Usage

  usage: pubfind [options]
  
  pubfind looks for missing publications. Args that start with '--' (eg.
  --fedora-api) can also be set in a config file (~/.pubfindrc or .pubfindrc or
  specified via -c). Config file syntax allows: key=value, flag=true,
  stuff=[a,b,c] (for details, see syntax at https://goo.gl/R74nmi). If an arg is
  specified in more than one place, then commandline values override config file
  values which override defaults.
  
  optional arguments:
    -h, --help            show this help message and exit
    -c CONFIG, --config CONFIG
                          config file path
    --fedora-api FEDORA_API
                          Fedora Commons API URL, usually https://.../fedora/
    --scopus-data SCOPUS_DATA
                          Scopus publications CSV data file
    --output-csv OUTPUT_CSV
                          Output CSV file

1. Download a CSV file from Scopus with all publications.

  1.1 Go to http://www.scopus.com/affil/ , search for an institution

      For example, University of New England, Australia is Scopus institution 60017837
  
  1.2 Click on 'Documents'
  This will take you to http://www.scopus.com/search/submit/affiliation.url?affiliationId=60017837&origin=AffiliationProfile
  The underlying search query for that is:
  
         AF-ID("University of New England Australia" 60017837) AND PUBYEAR IS 2015
  
  1.3 In the first column, select 'Select all' , as opposed to 'Select page'.
  If the total is less than 2000, download all information.
  
  1.4 Click Export, select CSV, and select "Citations and abstract information".

2. Run pubfind with the csv file

  $ pubfind --scopus-data=scopus_all_2015.csv --fedora-api="https://e-publications.une.edu.au/fedora"
  --------------.----.-.-..--..-.-.-.---.--.-----------------...---.--.--..------.-.-.---...-----------...-..-.--....----.---.-..---.--...-----.----.-.------.--.-..---..--.-----------------....----..-..-.--.-..---.---..--.--....-.-.-.----...--......-----.-..-..-...--..---..--...-.----...-..--.-.--..----..--.-.-.----..-.----..----.--...-..--.-.--...---..-....-..-----.-.-----.--.----.....----.--....-------.-.----.-.--.-..---.---..--..-....-..----...-..-..-..--..-.---.-..---.---.----.----.-...-.--...-...-.--.-..-.--..-.--..-----..--.-.....---.-..-----..--.--.-.....-..-.-..-----...--.-------.---..----.-.-....-.-.--.---.-....--.--.----.-.-..-.---....---.-.....--.-..-.----..------.-!
  pubfind.csv written

  ('-' means missing.  '.' means found.)

3. Analyse pubfind.csv which contains metadata for the missing publications, including citation counts.


# Adding datasources

Currently pubfind only obtains data from Scopus CSV files.

However the underlying data model is the CSL publication JSON data model,
which is specified using JSON Schema at https://github.com/citation-style-language/schema/blob/master/csl-data.json

That JSON schema is converted into objects using `csl-data`.
To add additional datasources, add readers to:

  https://github.com/jayvdb/csl-data
