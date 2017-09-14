# SPSS-Code
Practice SPSS Code

* Encoding: UTF-8.
*get relevant file and variables. 
GET FILE='w:\SPSS datasets\LFS household\PHHWT17\2017\AJ17.sav'
/KEEP phhwt17 age futype6 relhfu fuserial inecac05 ilodefr govtor sex ayfl19 fdpch19 sc10mmj soc10m hiqul15d likewk nolwm start ystart lktima ucredit oycirc ftpt yptjob redyl13 futwk tyemps.

SELECT IF (phhwt17>0).

*** REGION

RECODE govtor (1 THRU 16 =1) (17=2) (18 19=3) (20 = 4) into REGION.
VARIABLE LABELS REGION 'Region'.
VARIABLE LEVEL REGION (nominal).
VALUE LABELS REGION 1 'ENGLAND' 2 'WALES' 3 'SCOTLAND' 4 'N.IRELAND'. 
EXECUTE.

*recode for family units futype6.
RECODE futype6 (MISSING=0) (6, 9, 16, 19=3) (10, 12=2) (ELSE=1)  into futype1.
VARIABLE LABELS futype1 'Family Unit Type'.
VARIABLE LEVEL futype1 (NOMINAL).
VALUE LABELS futype1
1 'No Dependent Children'
2 'Lone Parents with dep children'
3 'Couples with dep children'
-99 'Missing'.
EXECUTE. 

RECODE ayfl19 (0 thru 2=1) (3 thru 4=2) (5 thru 10=3) (11 thru 15=4) (16 thru 18=5) into ayc.
VARIABLE LABELS ayc 'Age of Youngest Child'.
VARIABLE LEVEL ayc (NOMINAL).
VALUE LABELS ayc 
1 '0-2'
2 '3-4'
3 '5-10'
4 '11-15'
5 '16-18'.
EXECUTE. 

*Compute Parent Variable. 
COMPUTE parent=2.
IF ((relhfu=1|relhfu=2) & any(futype6, 6, 9, 10, 12, 16, 19)) parent=1.
VALUE LABELS parent 1 'Children' 2 'No Children'.
VARIABLE LEVEL parent (NOMINAL). 
EXECUTE.

*Compute FTPT Recode.
RECODE ftpt (1=1) (2=2) (3=1) (4=2) into ftptr.
VARIABLE LABELS ftptr 'Recode of ftpt'.
VARIABLE LEVEL ftptr (NOMINAL).
VALUE LABELS ftptr 
1 'Full Time'
2 'Part Time'
-9 'Missing'.
EXECUTE. 

*Compute type employment sought recode.
RECODE tyemps (1=1) (2=2) (3=3) (4 thru 5 =4) (6=2) (7=3) (8 thru 9=4) (10=-9) (11=5) into tyempsr.
VARIABLE LABELS tyempsr 'Recode of Type Employment Sought'.
VARIABLE LEVEL tyempsr (NOMINAL).
VALUE LABELS tyempsr 
1 'Self Employment'
2 'Full-Time Employment'
3 'Part-Time Employment'
4 'No Preference'
5 'Place on Government Scheme'
-9 'Missing'.
MISSING VALUES tyempsr (-9).
FORMATS tyempsr (f1.0).
EXECUTE. 

FREQUENCIES VARIABLES likewk nolwm start ystart lktima ucredit oycirc ftpt yptjob. 

