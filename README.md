# utl-plotting-significant-p-values-for-all-pairwise-treatment-comparisons-r-violin-graph
Plotting significant p values for all pairwise treatment comparisons r graph violin graph
    %let pgm=utl-plotting-significant-p-values-for-all-pairwise-treatment-comparisons-r-violin-graph;

    Plotting significant p values for all pairwise treatment comparisons r graph violin graph;


     hi res graphic output
     https://tinyurl.com/bdcuy5rx
     https://github.com/rogerjdeangelis/utl-plotting-significant-p-values-for-all-pairwise-treatment-comparisons-r-violin-graph/blob/main/pvals.pdf

     Solution by JPSmith
     https://stackoverflow.com/users/12109788/jpsmith

     stackoverflow R
     https://tinyurl.com/ttajjxzn
     https://stackoverflow.com/questions/79003181/calculating-pairwise-p-values-for-multiple-variables-in-r

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* CREATE A GRAPH SIIMILAR TO THIS                                                                                        */
    /*                                                                                                                        */
    /* IF YOU LOOK CLOSELY YOU CAN SEE UNIFORM DISTRIBUTIONS                                                                  */
    /*                                                                                                                        */
    /* Pairwise test Games-Howell significant Bars                                                                            */
    /*                                                                                                                        */
    /* Note the A-B p-value is not shown because it is not significant                                                        */
    /*                                                                                                                        */
    /*         FWelch(2, 192.73) = 11.38, p = 2.13e-05  Wp2 = 0.10, CI95% [0.04, 1.00], nobs = 300                            */
    /*                                                                                                                        */
    /*                                     DRUG                                                                               */
    /*                 A                     B                     C                                                          */
    /*               n=100                 n=100                 n=100                                                        */
    /*  RESULT   -----+--------------------- +--------------------+--------------------                                       */
    /*           |                                                                     | RESULT                               */
    /*           |                   p-holm-adj=0.0002                                 |                                      */
    /*           |     _____________________________________________                   |                                      */
    /*       160 +     |                                           |                   + 160                                  */
    /*           |         p-holm-adj=0.006                                            |                                      */
    /*           |     ________________________                                        |                                      */
    /*           |     |                      |                    |     14 5567       |                                      */
    /*       140 +                                                 |     14 344        + 140                                  */
    /*           |                                                 |     13 789        |                                      */
    /*           |                                                 |     13 01222      |                                      */
    /*           |                                                 |     12 5          |                                      */
    /*       120 +                            |    12 233          |     12 0123       + 120                                  */
    /*           |                            |    11 77899        |     11 68         |                                      */
    /*           |        STEM LEAF           |    11 11344     +-----+  11 034        |                                      */
    /*           |                            |    10 55689     |     |  10 55999      |                                      */
    /*       100 +                            |    10 3         |     |  10 111        + 100                                  */
    /*           |     |     9 56689          |     9 8         |     |   9 79         |                                      */
    /*           |     |     9 0004        +-----+  9 02233     |     |   9 03         |                                      */
    /*           |     |     8 689999      |     |  8 69        |     |   8 69         |                                      */
    /*        80 +     |     8 00111       |     |  8 112244    |  73 |   8 0124       +  80                                  */
    /*           |  +-----+  7 5568999     |     |  7 567779    |  +  |   7 577        |                                      */
    /*           |  |     |  7 11          |  64 |  7 112334    *-----*   7 0122       |                                      */
    /*           |  |     |  6 56677899    *--+--*  6 5679      |     |   6 6          |                                      */
    /*        60 +  |     |  6 134         |     |  6 00123     |     |   6 000114     +  60                                  */
    /*           |  |  50 |  5 5679        |     |  5 568       |     |   5 5999       |                                      */
    /*           |  |  +  |  5 134         |     |  5 11112     |     |   5 023        |                                      */
    /*           |  *-----*  4 5567788     |     |  4 67789     |     |   4 57         |                                      */
    /*        40 +  |     |  4 1112344     +-----+  4 00244     |     |   4 023        +  40                                  */
    /*           |  |     |  3 57788          |     3 8899      +-----+   3 6899       |                                      */
    /*           |  |     |  3 2234           |     3 0113334      |      3 02344      |                                      */
    /*           |  +-----+  2 57799          |     2 77789        |      2 55666      |                                      */
    /*        20 +     |     2 122334         |     2 33           |      2 13         +  20                                  */
    /*           |     |     1 5589           |     1 88899        |      1 556        |                                      */
    /*           |     |     1 00123344       |     1 014          |      1 0111       |                                      */
    /*           |     |     0 5599           |     0 8            |      0 78899      |                                      */
    /*         0 +     |     0 024            |     0 1            |      0 13         +   0                                  */
    /*            -----+----------------------+--------------------+--------------------                                      */
    /*      DRUG       A                      B                    C                                                          */
    /*               n=100                  n=100                 n=100                                                       */
    /*                                                                                                                        */
    /*                                      DRUG                                                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    data sd1.have;
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input drug $ result @@;
    cards4;
    A 029 A 079 A 041 A 088 A 094 A 005 A 053 A 089 A 055 A 046 A 096 A 045 A 068 A 057
    A 010 A 090 A 025 A 004 A 033 A 095 A 089 A 069 A 064 A 099 A 066 A 071 A 054 A 059
    A 029 A 015 A 096 A 090 A 069 A 080 A 002 A 048 A 076 A 022 A 032 A 023 A 014 A 041
    A 041 A 037 A 015 A 014 A 023 A 047 A 027 A 086 A 005 A 044 A 080 A 012 A 056 A 021
    A 013 A 075 A 090 A 037 A 067 A 009 A 038 A 027 A 081 A 045 A 081 A 081 A 079 A 044
    A 075 A 063 A 071 A 000 A 048 A 022 A 038 A 061 A 035 A 011 A 024 A 067 A 042 A 079
    A 010 A 043 A 098 A 089 A 089 A 018 A 013 A 065 A 034 A 066 A 032 A 019 A 078 A 009
    A 047 A 051 B 075 B 042 B 061 B 119 B 060 B 111 B 114 B 076 B 051 B 018 B 117 B 038
    B 008 B 118 B 090 B 018 B 069 B 119 B 073 B 051 B 081 B 040 B 038 B 027 B 046 B 123
    B 019 B 011 B 018 B 086 B 077 B 111 B 084 B 092 B 065 B 082 B 103 B 098 B 122 B 055
    B 039 B 051 B 001 B 023 B 105 B 029 B 030 B 010 B 031 B 092 B 106 B 062 B 048 B 031
    B 014 B 049 B 071 B 027 B 056 B 027 B 063 B 044 B 081 B 047 B 044 B 067 B 093 B 028
    B 052 B 033 B 079 B 023 B 108 B 093 B 084 B 077 B 047 B 066 B 109 B 073 B 105 B 039
    B 089 B 033 B 074 B 060 B 033 B 071 B 114 B 113 B 034 B 040 B 123 B 077 B 117 B 058
    B 051 B 082 B 019 B 072 C 036 C 144 C 090 C 077 C 060 C 132 C 055 C 043 C 026 C 026
    C 072 C 038 C 032 C 101 C 007 C 105 C 053 C 061 C 123 C 138 C 042 C 144 C 109 C 103
    C 008 C 059 C 072 C 084 C 105 C 137 C 093 C 064 C 081 C 009 C 039 C 060 C 030 C 125
    C 023 C 121 C 082 C 099 C 026 C 095 C 047 C 109 C 060 C 145 C 145 C 109 C 039 C 033
    C 089 C 040 C 080 C 118 C 025 C 061 C 071 C 130 C 139 C 132 C 101 C 143 C 077 C 086
    C 050 C 052 C 003 C 075 C 131 C 001 C 011 C 025 C 116 C 110 C 146 C 070 C 011 C 097
    C 114 C 021 C 059 C 034 C 009 C 059 C 010 C 034 C 008 C 101 C 045 C 015 C 011 C 132
    C 113 C 122 C 147 C 016 C 015 C 120
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*    SD1.HAVE total obs=300                                                                                              */
    /*                                                                                                                        */
    /*   Obs    DRUG    RESULT                                                                                                */
    /*                                                                                                                        */
    /*     1     A        29                                                                                                  */
    /*     2     A        79                                                                                                  */
    /*     3     A        41                                                                                                  */
    /*     4     A        88                                                                                                  */
    /*     5     A        94                                                                                                  */
    /*     ....                                                                                                               */
    /*                                                                                                                        */
    /*   296     C        122                                                                                                 */
    /*   297     C        147                                                                                                 */
    /*   298     C         16                                                                                                 */
    /*   299     C         15                                                                                                 */
    /*   300     C        120                                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    %utl_rbeginx;
    parmcards4;
    library(ggplot2)
    library(ggstatsplot)
    library(haven)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    set.seed(123)
    have <- data.frame(Drug = c(rep("A", 100), rep("B", 100), rep("C", 100)),
          Result = c(runif(100), runif(100, max = 1.25), runif(100, max = 1.5)))
    pdf("d:/pdf/pvals.pdf")
    ggbetweenstats(data = have, x = Drug, y = Result)
    pdf()
    ;;;;
    %utl_rendx;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
