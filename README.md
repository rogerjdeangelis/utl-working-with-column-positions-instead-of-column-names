# utl-working-with-column-positions-instead-of-column-names
Working with column position instead of column names.

    Working with column position instead of column names                                                        
                                                                                                                
    I need to rename the variables based on column position.                                                    
                                                                                                                
       Two Solutions                                                                                            
                                                                                                                
         1. SQL union by Bartosz Jablonski (append renamed tables)                                              
         2. Dosubl - ptoc datasets (renames individual tables but does no append)                               
                                                                                                                
         Trvial in R because columns can be addressed using tablename[,1],tablename[,2]                         
                                                                                                                
    see github and below                                                                                        
    http://tinyurl.com/ycqymkrl                                                                                 
    https://github.com/rogerjdeangelis/utl-working-with-column-positions-instead-of-column-names    
    
    SAS Forum:                                                                  
    http://tinyurl.com/y2zyn957                                                 
    https://communities.sas.com/t5/SAS-Programming/Rename-columns/m-p/538558    

                                                                                                                
    Bartosz Jablonski                                                                                           
    yabwon@gmail.com                                                                                            
                                                                                                                
    I inherited a dozen SAS tables from excel that have the same layout                                         
    but the column names are not the same. There three columns.                                                 
    The column names are hard to use.                                                                           
    I don't have access to the original excel files.                                                            
                                                                                                                
    You can also use Ian Whitlocks renamel macro.                                                               
                                                                                                                
    macros                                                                                                      
    https://tinyurl.com/y9nfugth                                                                                
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                  
                                                                                                                
    *          _               _                                                                                
     ___  __ _| |  _   _ _ __ (_) ___  _ __                                                                     
    / __|/ _` | | | | | | '_ \| |/ _ \| '_ \                                                                    
    \__ \ (_| | | | |_| | | | | | (_) | | | |                                                                   
    |___/\__, |_|  \__,_|_| |_|_|\___/|_| |_|                                                                   
            |_|                                                                                                 
    ;                                                                                                           
                                                                                                                
    ---------------------------------                                                                           
    1. SQL union by Bartosz Jablonski                                                                           
    --------------------------------                                                                            
                                                                                                                
    Bartosz Jablonski                                                                                           
    yabwon@gmail.com                                                                                            
                                                                                                                
    /*code*/                                                                                                    
    data hav_1;                                                                                                 
                                                                                                                
      set sashelp.class(obs=3 keep=name age sex rename=(                                                        
        name=nam_12_30_2018_03_04_53                                                                            
        sex=sex_12_30_2018_03_04_53                                                                             
        age=age_12_30_2018_03_04_53));                                                                          
                                                                                                                
    run;quit;                                                                                                   
    /*                                                                                                          
            Variables in Creation Order                                                                         
                                                                                                                
    #    Variable                   Type    Len                                                                 
                                                                                                                
    1    NAM_12_30_2018_03_04_53    Char      8                                                                 
    2    SEX_12_30_2018_03_04_53    Char      1                                                                 
    3    AGE_12_30_2018_03_04_53    Num       8                                                                 
    */                                                                                                          
                                                                                                                
    data hav_2;                                                                                                 
                                                                                                                
      set sashelp.class(obs=3 keep=name age sex rename=(                                                        
        name=nam_1_30_2019_05_07_13                                                                             
        sex=sex_1_30_2019_05_07_13                                                                              
        age=age_1_30_2019_05_07_13));                                                                           
                                                                                                                
    run;quit;                                                                                                   
                                                                                                                
    /*                                                                                                          
            Variables in Creation Order                                                                         
                                                                                                                
    #    Variable                  Type    Len                                                                  
                                                                                                                
    1    NAM_1_30_2019_05_07_13    Char      8                                                                  
    2    SEX_1_30_2019_05_07_13    Char      1                                                                  
    3    AGE_1_30_2019_05_07_13    Num       8                                                                  
    */                                                                                                          
                                                                                                                
    data hav_3;                                                                                                 
                                                                                                                
      set sashelp.class(obs=3 keep=name age sex rename=(                                                        
        name=xxxxxxxxx1                                                                                         
        sex=yyyyyyyyyyyy2                                                                                       
        age=zzzzzzzzz3));                                                                                       
                                                                                                                
    run;quit;                                                                                                   
                                                                                                                
    /*                                                                                                          
       Variables in Creation Order                                                                              
                                                                                                                
    #    Variable         Type    Len                                                                           
                                                                                                                
    1    XXXXXXXXX1       Char      8                                                                           
    2    YYYYYYYYYYYY2    Char      1                                                                           
    3    ZZZZZZZZZ3       Num       8                                                                           
    */                                                                                                          
                                                                                                                
                                                                                                                
    data shell;                                                                                                 
    set sashelp.class(keep=name age sex);                                                                       
    stop;                                                                                                       
    run;                                                                                                        
                                                                                                                
    /*                                                                                                          
     Variables in Creation Order                                                                                
                                                                                                                
    #    Variable    Type    Len                                                                                
                                                                                                                
    1    NAME        Char      8                                                                                
    2    SEX         Char      1                                                                                
    3    AGE         Num       8                                                                                
    */                                                                                                          
                                                                                                                
    proc sql;                                                                                                   
    create table want as                                                                                        
    select * from shell                                                                                         
    union all                                                                                                   
    select * from hav_1                                                                                         
    union all                                                                                                   
    select * from hav_2                                                                                         
    union all                                                                                                   
    select * from hav_3                                                                                         
    ;                                                                                                           
    quit;                                                                                                       
                                                                                                                
                                                                                                                
    /*                                                                                                          
     Variables in Creation Order                                                                                
                                                                                                                
    #    Variable    Type    Len                                                                                
                                                                                                                
    1    NAME        Char      8                                                                                
    2    SEX         Char      1                                                                                
    3    AGE         Num       8                                                                                
                                                                                                                
    Up to 40 obs from WANT total obs=9                                                                          
                                                                                                                
    Obs     NAME      SEX    AGE                                                                                
                                                                                                                
     1     Alfred      M      14                                                                                
     2     Alice       F      13                                                                                
     3     Barbara     F      13                                                                                
     4     Alfred      M      14                                                                                
     5     Alice       F      13                                                                                
     6     Barbara     F      13                                                                                
     7     Alfred      M      14                                                                                
     8     Alice       F      13                                                                                
     9     Barbara     F      13                                                                                
                                                                                                                
    *    _                 _     _                                                                              
      __| | ___  ___ _   _| |__ | |                                                                             
     / _` |/ _ \/ __| | | | '_ \| |                                                                             
    | (_| | (_) \__ \ |_| | |_) | |                                                                             
     \__,_|\___/|___/\__,_|_.__/|_|                                                                             
                                                                                                                
    ;                                                                                                           
                                                                                                                
    -------------------------                                                                                   
    2. Dosubl - ptoc datasets                                                                                   
    --------------------------                                                                                  
                                                                                                                
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
                                                                                                                



