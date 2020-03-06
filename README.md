# utl-using-proc-means-stackodsoutput-to-simplifiy-crosstab-percentages
Using proc means stackodsoutput to simplifiy crosstab percentages
    Using proc means stackodsoutput to simplifiy crosstab percentages

    github
    https://tinyurl.com/rzduhot
    https://github.com/rogerjdeangelis/utl-using-proc-means-stackodsoutput-to-simplifiy-crosstab-percentages

    SAS forum
    https://tinyurl.com/wlamze9
    https://communities.sas.com/t5/SAS-Programming/Report-table-with-missing-percentage-visit-treatment/m-p/627833

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    data have;
     input trt v1 v2 v3;
    cards4;
    1 2 3 .
    1 3 4 .
    2 . . .
    2 4 . .
    ;;;;
    run;quit;


    WORK.HAVE total obs=4

      TRT    V1    V2    V3

       1      2     3     .
       1      3     4     .

       2      .     .     .
       2      4     .     .


    *           _
     _ __ _   _| | ___  ___
    | '__| | | | |/ _ \/ __|
    | |  | |_| | |  __/\__ \
    |_|   \__,_|_|\___||___/

    ;

    TRT         V1    V2    V3

     1           2     3     .
     1           3     4     .

    ToT MISS     0     0     4

    PCT MISS     0%    0%   100%  * this is what we want

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;


    WORK.WANT total obs=3

     VARIABLE     TRT1     TRT2

        V1         0.0%    50.0%
        V2         0.0%     0.0%
        V3       100.0%     0.0%

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    proc means data=have stackodsoutput chartype n nmiss;
      by trt;
      var v1 v2 v3 ;
      ods output summary=havSum;
    run;quit;


    data want;
      format variable $8. trt1 trt2 percent8.1;
      merge havSum(where=(trt=1))
            havSum(where=(trt=2) rename=(n=n2 nmiss=nm2));
      trt1=nmiss/(n+nmiss);
      trt2=n2/(n2+nm2);
      keep variable trt1 trt2;
    run;quit;


