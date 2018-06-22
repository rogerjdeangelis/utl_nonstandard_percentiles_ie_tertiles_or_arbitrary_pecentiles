# utl_nonstandard_percentiles_ie_tertiles_or_arbitrary_pecentiles
Calculate the first tertile or arbitrary percentiles.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Calculate the first Tertile or arbitrary percentiles;

    Same results in Base SAS, Base WPS and

    For nonstandard percentiles, use WPS/SAS proc stdize

      Two Solutions

          1. WPS and SAS proc stdize
          2. WPS Proc R (IML/R or WPS/R 'call qntl(Pctls, X, Prob)' not run)

    github
    https://tinyurl.com/yd9d2cs6
    https://github.com/rogerjdeangelis/utl_nonstandard_percentiles_ie_tertiles_or_arbitrary_pecentiles

    see SAS Forum
    https://tinyurl.com/yamnwca6
    https://communities.sas.com/t5/SAS-Statistical-Procedures/Tertile-in-SAS-mean/m-p/472386

    https://blogs.sas.com/content/iml/2013/10/23/percentiles-in-a-tabular-format.html


    INPUT (10 thousand uniform random variates)
    ===========================================

     SD1.HAVE total obs=10,000

      Obs       X

       1    0.24381
       2    0.08948
       3    0.38319
       4    0.09793
       5    0.25758
       6    0.08825
     ...


    PROCESS
    =======

     1. proc stdize (WPS and SAS)

        proc stdize data=sd1.have PctlMtd=ORD_STAT outstat=want
               pctlpts=%sysevalf(100/3);
           var x;
        run;quit;

        WARNING: Percentile 33.333333333 is truncated to 33.3333.


     2. WPS Proc R quantile

        want<-quantile(have$X, prob = c(1/3));

        IML 'call qntl(Pctls, X, Prob)'  not run

    OUTPUT
    ======

       1. WPS/SAS

          Obs    _TYPE_                       X

           1     LOCATION        0.497338991021
           2     SCALE           0.288527043786
           3     ADD             0.000000000000
           4     MULT            1.000000000000
           5     N           10000.000000000000
           6     NObsRead    10000.000000000000
           7     NObsUsed    10000.000000000000
           8     NObsMiss        0.000000000000
           9     P33_3333        0.329961552904

       2. WPS/Proc R

          Obs                  WANT

           1         0.329961552904


    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    For SAS see Process

    * WPS;

    %utl_submit_wps64('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname sd1 "d:/sd1";
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    proc stdize data=sd1.have pctlmtd=ord_stat outstat=wrk.want_std
           pctlpts=%sysevalf(100/3);
       var x;
    run;quit;
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    want<-quantile(have$X, prob = c(1/3));
    endsubmit;
    import r=want  data=wrk.want_r;
    run;quit;
    ');

    proc print data=want_r;
    format want 18.12;
    run;quit;

    proc print data=want_std;
    format x 18.12;
    run;quit;


