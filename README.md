# 2017-summer
summer intern/project 

week 1 - identifying neoplasm patients in mimic3 data, mapping to UMLS CUI. 
        `CUI means concept unique identity, such as ICD code, SNOMED-CT.`

Steps: Load D_ICD_DIAGNOSES.csv;
        Search the icd9data.com website to see what codes relates to NEoplasms, found 140-239.http://www.icd9data.com/2015/Volume1/140-239/default.htm

        Select icd9_id in that range from the full dataset, nothing found.

        Select text 'neoplasm', found 1088 total patients. 

Q: On the mimic dataset, it seems the icd9_id doesn't match icd9data.com either.


        

week2 - tested NLTK tokenization, comparing it with regular expressions; 

      -read bs4 documents and tried to figure how it works with lxml packages, 
such as `lxml.html.soupparser`. please see http://lxml.de/intro.html

      -We have built python script to parse SNOMEDCT, etc bioportal purl, 

    Q: I couldn't retrieve any relative ICD10CM, ICD10, or ICD9CM for the keyword 'neoplasm'. 

