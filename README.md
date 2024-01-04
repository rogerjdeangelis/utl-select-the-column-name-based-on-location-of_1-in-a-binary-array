# utl-select-the-column-name-based-on-location-of_1-in-a-binary-array
Select the column name based on location of 1 in a binary array 
%let pgm=utl-select-the-column-name-based-on-location-of_1-in-a-binary-array;

Select the column name based on location of 1 in a binary array

github
http://tinyurl.com/36k2b5j9
https://github.com/rogerjdeangelis/utl-select-the-column-name-based-on-location-of_1-in-a-binary-array

stackoverflow
http://tinyurl.com/576cvh9c
https://stackoverflow.com/questions/77756573/collapse-multiple-columns-into-one-column-based-on-values-in-r

SOLUTIONS

   1. wps hardcoded case when
   2. wps dynamic case when
   3. wps r dynamic case when
   4. wps python dynamic case when
   5. wps base
   6  wps r basic

/**************************************************************************************************************************/
/*                                             |                                              |                           */
/*                INPUT                        |                 PROCESS                      |     OUTPUT                */
/*                =====                        |                 ========                     |     =====                 */
/*                                             |                                              |                           */
/* SD1.HAVE total obs=10                       |  create                                      |                           */
/*                                             |     table want as                            |                           */
/* Obs    APPLE    BANANA    CHERRY    DATE    |  select                                      |   Obs    FRUIT            */
/*                                             |     case                                     |                           */
/*   1      0         1         0        0     |        when (APPLE  =1) then "APPLE"         |     1    BANANA           */
/*   2      0         0         1        0     |        when (BANANA =1) then "BANANA"        |     2    CHERRY           */
/*   3      0         0         0        1     |        when (CHERRY =1) then "CHERRY"        |     3    DATE             */
/*   4      1         0         0        0     |        when (DATE   =1) then "DATE"          |     4    APPLE            */
/*   5      0         1         0        0     |        else "ERROR"                          |     5    BANANA           */
/*   6      1         0         0        0     |     end as Fruit                             |     6    APPLE            */
/*   7      0         1         0        0     |  from                                        |     7    BANANA           */
/*   8      0         0         1        0     |     sd1.have                                 |     8    CHERRY           */
/*   9      0         0         0        1     |                                              |     9    DATE             */
/*  10      1         0         0        0     |           DYNAMIC ARRAY                      |    10    APPLE            */
/*                                             |           =============                      |                           */
/*                                             |                                              |                           */
/*                                             |  %array(_ns,values=%utl_varlist(sd1.have));  |                           */
/*                                             |                                              |                           */
/*                                             |  proc sql;                                   |                           */
/*                                             |    create                                    |                           */
/*                                             |       table sd1.want as                      |                           */
/*                                             |    select                                    |                           */
/*                                             |       case                                   |                           */
/*                                             |          %do_over(_nm,phrase=%str(           |                           */
/*                                             |            when (? =1) then '?'))            |                           */
/*                                             |            else 'ERROR'                      |                           */
/*                                             |       end as Fruit                           |                           */
/*                                             |    from                                      |                           */
/*                                             |       sd1.have                               |                           */
/*                                             |  ;quit;                                      |                           */
/**************************************************************************************************************************/

 /*                   _
(_)_ __  _ __  _   _| |_
| | `_ \| `_ \| | | | __|
| | | | | |_) | |_| | |_
|_|_| |_| .__/ \__,_|\__|
        |_|
*/

options validvarname=upcase;
libname sd1 "d:/sd1";
data sd1.have;
  input apple banana cherry date ;
cards4;
0 1 0 0
0 0 1 0
0 0 0 1
1 0 0 0
0 1 0 0
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 1
1 0 0 0
;;;;
run;quit;

/**************************************************************************************************************************/
/*                                                                                                                        */
/*  Obs    APPLE    BANANA    CHERRY    DATE                                                                              */
/*                                                                                                                        */
/*    1      0         1         0        0                                                                               */
/*    2      0         0         1        0                                                                               */
/*    3      0         0         0        1                                                                               */
/*    4      1         0         0        0                                                                               */
/*    5      0         1         0        0                                                                               */
/*    6      1         0         0        0                                                                               */
/*    7      0         1         0        0                                                                               */
/*    8      0         0         1        0                                                                               */
/*    9      0         0         0        1                                                                               */
/*   10      1         0         0        0                                                                               */
/*                                                                                                                        */
/**************************************************************************************************************************/


/*                    _                   _               _          _                                 _
__      ___ __  ___  | |__   __ _ _ __ __| | ___ ___   __| | ___  __| |   ___ __ _ ___  ___  __      _| |__   ___ _ __
\ \ /\ / / `_ \/ __| | `_ \ / _` | `__/ _` |/ __/ _ \ / _` |/ _ \/ _` |  / __/ _` / __|/ _ \ \ \ /\ / / `_ \ / _ \ `_ \
 \ V  V /| |_) \__ \ | | | | (_| | | | (_| | (_| (_) | (_| |  __/ (_| | | (_| (_| \__ \  __/  \ V  V /| | | |  __/ | | |
  \_/\_/ | .__/|___/ |_| |_|\__,_|_|  \__,_|\___\___/ \__,_|\___|\__,_|  \___\__,_|___/\___|   \_/\_/ |_| |_|\___|_| |_|
         |_|
*/

proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

%utl_submit_wps64x("
libname sd1 'd:/sd1';
options validvarname=any;
proc sql;
  create
     table sd1.want as
  select
     case
        when (APPLE  =1) then 'APPLE'
        when (BANANA =1) then 'BANANA'
        when (CHERRY =1) then 'CHERRY'
        when (DATE   =1) then 'DATE'
        else "ERROR"
     end as Fruit
  from
     sd1.have
;quit;
");

proc print data=sd1.want;
run;quit;

/*                        _                                                                       _
__      ___ __  ___    __| |_   _ _ __   __ _ _ __ ___   ___   ___   ___ __ _ ___  ___  __      _| |__   ___ _ __
\ \ /\ / / `_ \/ __|  / _` | | | | `_ \ / _` | `_ ` _ \ / _ \ / __| / __/ _` / __|/ _ \ \ \ /\ / / `_ \ / _ \ `_ \
 \ V  V /| |_) \__ \ | (_| | |_| | | | | (_| | | | | | | (_) | (__ | (_| (_| \__ \  __/  \ V  V /| | | |  __/ | | |
  \_/\_/ | .__/|___/  \__,_|\__, |_| |_|\__,_|_| |_| |_|\___/ \___| \___\__,_|___/\___|   \_/\_/ |_| |_|\___|_| |_|
         |_|                |___/
*/

%array(_ns,values=%utl_varlist(sd1.have));

%put &=_nsn;

%utl_submit_wps64x("
libname sd1 'd:/sd1';
options validvarname=any;
proc sql;
  create
     table sd1.want as
  select
     case
        %do_over(_nm,phrase=%str(
          when (? =1) then '?'))
          else 'ERROR'
     end as Fruit
  from
     sd1.have
;quit;
");

proc print data=sd1.want;
run;quit;

/*                               _                             _                                      _
__      ___ __  ___   _ __    __| |_   _ _ __   __ _ _ __ ___ (_) ___    ___ __ _ ___  ___  __      _| |__   ___ _ __
\ \ /\ / / `_ \/ __| | `__|  / _` | | | | `_ \ / _` | `_ ` _ \| |/ __|  / __/ _` / __|/ _ \ \ \ /\ / / `_ \ / _ \ `_ \
 \ V  V /| |_) \__ \ | |    | (_| | |_| | | | | (_| | | | | | | | (__  | (_| (_| \__ \  __/  \ V  V /| | | |  __/ | | |
  \_/\_/ | .__/|___/ |_|     \__,_|\__, |_| |_|\__,_|_| |_| |_|_|\___|  \___\__,_|___/\___|   \_/\_/ |_| |_|\___|_| |_|
         |_|                       |___/

*/

proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

%array(_ns,values=%utl_varlist(sd1.have));

libname sd1 "d:/sd1";

%utl_submit_wps64x("
libname sd1 'd:/sd1';
proc r;
export data=sd1.have r=have;
submit;
library(sqldf);
want<-sqldf('
  select
      case
        %do_over(_ns,phrase=%str(
          when ? =1.0 then \'?\'))
          else \'ERR\'
     end as Fruit
  from
     have
');
want;
endsubmit;
import data=sd1.want r=want;
run;quit;
");

/**************************************************************************************************************************/
/*                                                                                                                        */
/* The WPS R System                                                                                                       */
/*                                                                                                                        */
/*  $ APPLE : num  0 0 0 1 0 1 0 0 0 1                                                                                    */
/*  $ BANANA: num  1 0 0 0 1 0 1 0 0 0                                                                                    */
/*  $ CHERRY: num  0 1 0 0 0 0 0 1 0 0                                                                                    */
/*  $ DATE  : num  0 0 1 0 0 0 0 0 1 0                                                                                    */
/*                                                                                                                        */
/*     Fruit                                                                                                              */
/* 1  BANANA                                                                                                              */
/* 2  CHERRY                                                                                                              */
/* 3    DATE                                                                                                              */
/* 4   APPLE                                                                                                              */
/* 5  BANANA                                                                                                              */
/* 6   APPLE                                                                                                              */
/* 7  BANANA                                                                                                              */
/* 8  CHERRY                                                                                                              */
/* 9    DATE                                                                                                              */
/* 10  APPLE                                                                                                              */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*                                      _                             _                                      _
__      ___ __  ___   _ __  _   _    __| |_   _ _ __   __ _ _ __ ___ (_) ___    ___ __ _ ___  ___  __      _| |__   ___ _ __
\ \ /\ / / `_ \/ __| | `_ \| | | |  / _` | | | | `_ \ / _` | `_ ` _ \| |/ __|  / __/ _` / __|/ _ \ \ \ /\ / / `_ \ / _ \ `_ \
 \ V  V /| |_) \__ \ | |_) | |_| | | (_| | |_| | | | | (_| | | | | | | | (__  | (_| (_| \__ \  __/  \ V  V /| | | |  __/ | | |
  \_/\_/ | .__/|___/ | .__/ \__, |  \__,_|\__, |_| |_|\__,_|_| |_| |_|_|\___|  \___\__,_|___/\___|   \_/\_/ |_| |_|\___|_| |_|
         |_|         |_|    |___/         |___/
*/

proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

%utl_submit_wps64x("
options validvarname=any lrecl=32756;
libname sd1 'd:/sd1';
proc python;
export data=sd1.have python=have;
submit;
import pyreadstat;
from os import path;
import pandas as pd;
import numpy as np;
from pandasql import sqldf;
mysql = lambda q: sqldf(q, globals());
from pandasql import PandaSQL;
pdsql = PandaSQL(persist=True);
want = pdsql('''
  select
      case
        %do_over(_ns,phrase=%str(
          when ? =1.0 then \'?\'))
          else \'ERR\'
     end as Fruit
  from
     have
''');
print(want);
pyreadstat.write_xport(want, 'd:/xpt/want.xpt',table_name='want',file_format_version=5);
endsubmit;
run;quit;
");

libname xpt xport "d:/xpt/want.xpt";
proc contents data=xpt._all_;
run;quit;
data want;
  set xpt.want;
run;quit;
proc print;
run;quit;


/**************************************************************************************************************************/
/*                                                                                                                        */
/* The WPS System                                                                                                         */
/*                                                                                                                        */
/* The PYTHON Procedure                                                                                                   */
/*                                                                                                                        */
/*     Fruit                                                                                                              */
/* 0  BANANA                                                                                                              */
/* 1  CHERRY                                                                                                              */
/* 2    DATE                                                                                                              */
/* 3   APPLE                                                                                                              */
/* 4  BANANA                                                                                                              */
/* 5   APPLE                                                                                                              */
/* 6  BANANA                                                                                                              */
/* 7  CHERRY                                                                                                              */
/* 8    DATE                                                                                                              */
/* 9   APPLE                                                                                                              */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*                    _
__      ___ __  ___  | |__   __ _ ___  ___
\ \ /\ / / `_ \/ __| | `_ \ / _` / __|/ _ \
 \ V  V /| |_) \__ \ | |_) | (_| \__ \  __/
  \_/\_/ | .__/|___/ |_.__/ \__,_|___/\___|
         |_|
*/

proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

%utl_submit_wps64x("
libname sd1 'd:/sd1';
data sd1.want;
 set sd1.have;
 array names %utl_varlist(have);
 index=find(cats(of _numeric_),'1');
 fruit=vname(names[index]);
run;quit;
");

/**************************************************************************************************************************/
/*                                                                                                                        */
/* Obs    APPLE    BANANA    CHERRY    DATE    INDEX    FRUIT                                                             */
/*                                                                                                                        */
/*   1      0         1         0        0       2      BANANA                                                            */
/*   2      0         0         1        0       3      CHERRY                                                            */
/*   3      0         0         0        1       4      DATE                                                              */
/*   4      1         0         0        0       1      APPLE                                                             */
/*   5      0         1         0        0       2      BANANA                                                            */
/*   6      1         0         0        0       1      APPLE                                                             */
/*   7      0         1         0        0       2      BANANA                                                            */
/*   8      0         0         1        0       3      CHERRY                                                            */
/*   9      0         0         0        1       4      DATE                                                              */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*                           _
__      ___ __  ___   _ __  | |__   __ _ ___  ___
\ \ /\ / / `_ \/ __| | `__| | `_ \ / _` / __|/ _ \
 \ V  V /| |_) \__ \ | |    | |_) | (_| \__ \  __/
  \_/\_/ | .__/|___/ |_|    |_.__/ \__,_|___/\___|
         |_|
*/

proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

%utl_submit_wps64("
libname sd1 'd:/sd1';
proc r;
export data=sd1.have r=df1;
submit;
want<-(res <- data.frame(fruit=names(df1)[max.col(df1)]) |> `rownames<-`(rownames(df1)));
want;
endsubmit;
import data=sd1.want r=want;
");

/**************************************************************************************************************************/
/*                                                                                                                        */
/* The WPS R System                                                                                                       */
/*                                                                                                                        */
/*     fruit                                                                                                              */
/* 1  BANANA                                                                                                              */
/* 2  CHERRY                                                                                                              */
/* 3    DATE                                                                                                              */
/* 4   APPLE                                                                                                              */
/* 5  BANANA                                                                                                              */
/* 6   APPLE                                                                                                              */
/* 7  BANANA                                                                                                              */
/* 8  CHERRY                                                                                                              */
/* 9    DATE                                                                                                              */
/* 10  APPLE                                                                                                              */
/*                                                                                                                        */
/**************************************************************************************************************************/

/*              _
  ___ _ __   __| |
 / _ \ `_ \ / _` |
|  __/ | | | (_| |
 \___|_| |_|\__,_|

*/
