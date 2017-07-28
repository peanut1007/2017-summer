# 2017-summer
summer intern/project 

week 1 
- identifying neoplasm patients in mimic3 data, mapping to UMLS CUI. 
    `CUI means concept unique identifier, such as ICD code, SNOMED-CT. ` 
    https://www.nlm.nih.gov/research/umls/new_users/glossary.html

        Steps: 
        Load D_ICD_DIAGNOSES.csv;
        Search the icd9data.com website to see what codes relates to NEoplasms, found 140-239.
        http://www.icd9data.com/2015/Volume1/140-239/default.htm

        Select icd9_id in that range from the full dataset, nothing found.

        Select text 'neoplasm', found 1088 total patients. please see the ipynb. 

Q: On the mimic dataset, it seems the icd9_id doesn't match icd9data.com either.


        

week2 
- tested NLTK tokenization, comparing it with regular expressions; 

- read bs4 documents and tried to figure how it works with lxml packages, 
such as `lxml.html.soupparser`. please see http://lxml.de/intro.html

- We have built python script to parse SNOMEDCT, etc bioportal purl, 

Q: I couldn't retrieve any relative ICD10CM, ICD10, or ICD9CM for the keyword 'neoplasm'. 
A: Add period "." after the third digit to match ICD9 code.

week3 

- ICD9: diag_icd JOIN D_icd_dignoses table ON ICD9_CODE(key)
- Medication: prescription table
- Lab Test: labe event table JOIN D_labitem table ON d_labitem(key)

SQL code :
1. select icd9_code, long_title from d_icd_diagnoses where long_title like '%neoplasm%'

2. select subject_id,  hadm_id,  left_t.icd9_code , long_title
from diagnoses_icd as left_t
left join d_icd_diagnoses as right_t
on left_t.icd9_code = right_t.icd9_code
where long_title like '%neoplasm%'
order by subject_id asc, icd9_code asc

3. select distinct (subject_id, left_t.icd9_code) , long_title
from diagnoses_icd as left_t
left join d_icd_diagnoses as right_t
on left_t.icd9_code = right_t.icd9_code
where long_title like '%neoplasm%'

4. Test Results
select subject_id,  hadm_id,  left_t.itemid, label, valuenum, valueuom, flag
from labevents as left_t
left join d_labitems as right_t
on left_t.itemid = right_t.itemid

5. select * from (select subject_id,  hadm_id,  left_t.itemid, label, valuenum, valueuom, flag
from labevents as left_t
left join d_labitems as right_t
on left_t.itemid = right_t.itemid) as left_q
left join 
(select subject_id,  hadm_id,  left_t.icd9_code , long_title
from diagnoses_icd as left_t
left join d_icd_diagnoses as right_t
on left_t.icd9_code = right_t.icd9_code
where long_title like '%neoplasm%'
order by subject_id asc, icd9_code asc
) as right_q
on left_q.subject_id = right_q.subject_id
