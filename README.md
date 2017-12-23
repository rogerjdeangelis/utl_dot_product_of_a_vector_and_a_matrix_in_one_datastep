# utl_dot_product_of_a_vector_and_a_matrix_in_one_datastep
Dot product of a vector and a matrix in one datastep.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning.
    SAS Forum: Dot product of a vector and a matrix in one datastep

     WPS/SAS solutions

      FYI (a one liner id R or IML)  "R  want <- L %*% O"

    Original topic
      For each 'key' multiply matrices and store the output corresponding to the key


    INPUT
    =====

     WORK.HAVE total obs=2

      KEY     T12    T13    T11    T21    T23    T22    T31    T32    T33

      key1    0.2    0.6    0.2    0.3    0.6    0.1    0.1    0.3    0.6
      key2    0.1    0.5    0.4    0.2    0.6    0.2    0.2    0.3    0.5

     WORK.KEY total obs=2

       KEY     L1    L2    L3

       key1    20    10    12
       key2    23     5     9

     RULES
     =====
                     T11 T12 T13              0.2
        L1 L2 L3  o  T21 T22 T23  20 10 12  o 0.3  = 4 + 3 + 1.2 = 8.2
                     T23 T32 T33              0.1


    PROCESS (all the code WPS/SAS)
    ==============================

        * it is always the meta data and the data structure that is key;
        data want(keep=w1-w3);
             merge have key;
             array hav[3,3] t:;
             array els[3] l1-l3;
             array tre[3] w1-w3;
             array ord[3,3] o1-o9;

             * put the arraty in order;
             do i=1 to 3;
               do j=1 to 3;
                 x=input(substr(vname(hav[i,j]),2,1),2.);
                 y=input(substr(vname(hav[i,j]),3,1),2.);
                 ord[x,y]=hav[i,j];
               end;
             end;

             * compute dot product;
             do i=1 to 3;
               do j=1 to 3;
                 *put els[j]= ord[i,j]=;
                 tre[i]=sum(tre[i],els[j]*ord[j,i]);
               end;
             end;
        run;quit;

    OUTPUT
    ======

       The WOS System

       WORK.WANT total obs=2

          W1      W2     W3

          8.2    8.6    25.2
         12.0    6.0    19.0

    see
    https://goo.gl/nnwNX7
    https://communities.sas.com/t5/SAS-IML-Software-and-Matrix/For-each-key-multiply-matrices-and-store-the-output/m-p/421938


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";

    data sd1.have;
    input key$
          t12 t13 t11 t21 t23 t22 t31 t32 t33;
    cards4;
    key1 0.2 0.6 0.2 0.3 0.6 0.1 0.1 0.3 0.6
    key2 0.1 0.5 0.4 0.2 0.6 0.2 0.2 0.3 0.5
    ;;;;
    run;quit;

    data sd1.key;
    input key$ l1 l2 l3 ;
    cards4;
    key1 20 10 12
    key2 23 5 9
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    libname wrk "%sysfunc(pathname(work))";
    data wrk.wantwps(keep=w1-w3);
         merge sd1.have sd1.key;
         array hav[3,3] t:;
         array els[3] l1-l3;
         array tre[3] w1-w3;
         array ord[3,3] o1-o9;
         do i=1 to 3;
           do j=1 to 3;
             x=input(substr(vname(hav[i,j]),2,1),2.);
             y=input(substr(vname(hav[i,j]),3,1),2.);
             ord[x,y]=hav[i,j];
           end;
         end;
         do i=1 to 3;
           do j=1 to 3;
             *put els[j]= ord[i,j]=;
             tre[i]=sum(tre[i],els[j]*ord[j,i]);
           end;
         end;
    run;quit;
    proc print;
    run;quit;
    ');

    proc print data=wantwps;
    run;quit;

