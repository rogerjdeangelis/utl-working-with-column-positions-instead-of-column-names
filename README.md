# utl-working-with-column-positions-instead-of-column-names
Working with column position instead of column names.

    Working with column position instead of column names

    I need to rename the variables based on column position.

    I inherited a dozen SAS tables from excel that have the same layout
    but the column names are not the same. There three columns.
    The column names are hard to use.
    I don't have access to the original excel files.

    You can also use Ian Whitlocks renamel macro.

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    INPUT
    =====

    data hav_1;

      set sashelp.class(obs=3 keep=name age sex rename=(
        name=nam_12_30_2018_03_04_53
        sex=sex_12_30_2018_03_04_53
        age=age_12_30_2018_03_04_53));

    run;quit;

    data hav_2;

      set sashelp.class(obs=3 keep=name age sex rename=(
        name=nam_1_30_2019_05_07_13
        sex=sex_1_30_2019_05_07_13
        age=age_1_30_2019_05_07_13));

    run;quit;

    ....

    WORK.HAV_1 total obs=3

      NAM_12_30_    GENDER_12_    AGE_IN_YEARS_
       2019_03_      30_2019_      12_30_2019_
        04_53        03_04_53        03_04_53

       Alfred           M               14
       Alice            F               13
       Barbara          F               13

    WORK.HAV_2 total obs=3

       NAM_1_30_    SEX_1_30_    AGE_1_30_
       2019_05_     2019_05_      2019_05_
        07_13        07_13        07_13

       Alfred          M            14
       Alice           F            13
       Barbara         F            13


    EXAMPLE OUTPUT
    --------------

    WORK.LOG total obs=2

       DSNS     RC         STATUS

       hav_1     0    rename sucessful
       hav_2     0    rename sucessful


    WORK.HAV_1 total obs=3

       NAME      SEX    AGE

      Alfred      M      14
      Alice       F      13
      Barbara     F      13


    WORK.HAV_2 total obs=3

       NAME      SEX    AGE

      Alfred      M      14
      Alice       F      13
      Barbara     F      13


    PROCESS ( You can use do_over to do all columns)
    ================================================

    data log1;

       do dsns='hav_1','hav_2';

          call symputx('dsn',dsns);

          rc=dosubl('

             %array(c,values=%utl_varlist(&dsn));

             proc datasets lib=work;
             modify &dsn;
                rename
                  &c1 = name
                  &c2 = sex
                  &c3 = age
             ;
          run;quit;
          %let cc=&syserr;
         ');

         if symgetn("cc")=0 then status="rename sucessful";
         else status="rename failed";
         output;

       end;

    run;quit;


    Output see above;
    =================


