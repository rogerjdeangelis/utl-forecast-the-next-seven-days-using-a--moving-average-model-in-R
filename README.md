# utl-forecast-the-next-seven-days-using-a--moving-average-model-in-R
Forecast the next seven days using a moving average model in R 
    Forecast the next seven days using a moving average model in R                                                       
                                                                                                                         
    github                                                                                                               
    https://tinyurl.com/ycmnaolw                                                                                         
    https://github.com/rogerjdeangelis/utl-forecast-the-next-seven-days-using-a--moving-average-model-in-R               
                                                                                                                         
    It has been a while since I did time series forcasting so be skeptical.                                              
                                                                                                                         
    Your data appears to be random noise. I added a column with a weak 7 day structure with random noise;                
    You can also run the model with your data just delete my visit statement.                                            
                                                                                                                         
                                                                                                                         
                     7      14     21      28   35                                                                       
            --+---------+---------+---------+---------+-                                                                 
     VISITS |        |      |      |      |      |     |                                                                 
        160 +        |      |      |      |      |     +  160                                                            
            |        |  * * |      |      |      |     |                                                                 
            |        |      |     *|      |      |     |                                                                 
            |      * |      |      |      |      |     |                                                                 
            |        |      |    * |      |      |     |                                                                 
            |    **  |      |      |      |*  *  |     |                                                                 
        140 +       *|      |      |      |    **|     +  140                                                            
            |        |      |  *   |      | *    |     |                                                                 
            |        |      |      |      |      |     |                                                                 
            |        |   * *|      *    * *  *   |     |                                                                 
            |        **     |      | *    |      *  *  |                                                                 
            |        |      |      |   *  |      |     |                                                                 
        120 +        |      |      |     *|      |     +  120                                                            
            |        | *    |** *  |* *   |      |     |                                                                 
            |  **    |      |      |      |      |     |                                                                 
            |        |      |      |      |      |     |                                                                 
            |        |      *      |      |      |**   |                                                                 
            |        |      |      |      |      |     |                                                                 
        100 +        |      |      |      |      |     +  100                                                            
            --+---------+---------+---------+---------+-                                                                 
              0        10        20        30        40                                                                  
                                                                                                                         
                                 SEQ                                                                                     
                                                                                                                         
    I added some structure  (see code below)                                                                             
                                DAY                                                                                      
                                                                                                                         
                    7      14     21    28      35                                                                       
           --+---------+---------+---------+---------+-                                                                  
    VISITS |        |      |      |      |      |     |                                                                  
        60 +        |      |      |      |      |     +                                                                  
           |        |      *      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |        *      |      |      |      *     |                                                                  
           |        |      |      *      *      |     |                                                                  
        40 +        |      |      |     *|      |     +                                                                  
           |        |     *|      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |       *|      |      |      |     *|     |                                                                  
           |        |      |      |    * |      |     |                                                                  
           |        |      |   * *|   *  |    * |     |                                                                  
        20 +      * |   *  |    * |      |      |     +                                                                  
           |     *  |    * |      |  *   |   *  |  *  |                                                                  
           |        |      |  *   |      |  *   |     |                                                                  
           |   **   | **   | *    |      |      | *   |                                                                  
           |        |      |      | *    | *    |     |                                                                  
           |        |      |      |      |      |     |                                                                  
         0 +  *     |*     |*     |*     |*     |*    +                                                                  
           --+---------+---------+---------+---------+-                                                                  
             0        10        20        30        40                                                                   
                                                                                                                         
                                DAY                                                                                      
                                                                                                                         
                                                                                                                         
    This appears to be similar to                                                                                        
                                                                                                                         
    graph                                                                                                                
    https://tinyurl.com/y6yppq3f                                                                                         
    https://github.com/rogerjdeangelis/utl-forecast-the-next-four-months-using-a-moving-average-time-series/blob/master/f
                                                                                                                         
    github                                                                                                               
    https://tinyurl.com/y3g7gpfj                                                                                         
    https://github.com/rogerjdeangelis/utl-forecast-the-next-four-months-using-a-moving-average-time-series              
                                                                                                                         
    SAS Fotum                                                                                                            
    https://tinyurl.com/yxzzmszt                                                                                         
    https://communities.sas.com/t5/SAS-Programming/How-to-create-new-rows-with-moving-average-calculation/m-p/591711     
                                                                                                                         
    /*                   _                                                                                               
    (_)_ __  _ __  _   _| |_                                                                                             
    | | `_ \| `_ \| | | | __|                                                                                            
    | | | | | |_) | |_| | |_                                                                                             
    |_|_| |_| .__/ \__,_|\__|                                                                                            
            |_|                                                                                                          
    */                                                                                                                   
    Here is a centered moving average model                                                                              
                                                                                                                         
    options validvarname=upcase;                                                                                         
    libname sd1 "d:/sd1";                                                                                                
                                                                                                                         
    data sd1.have(keep = date visits );                                                                                  
    input DATE date9. VISITS ADMITS period;                                                                              
       day=_n_;                                                                                                          
       /* I added this */                                                                                                
       visits= 20*mod(_n_-1,7) - 1*visits*mod(_n_-1,7)/10;                                                               
    cards4;                                                                                                              
    01MAY2002 113 17 2 113                                                                                               
    02MAY2002 112 11 2 112                                                                                               
    03MAY2002 142 16 2 142                                                                                               
    04MAY2002 143 14 2 143                                                                                               
    05MAY2002 150 11 3 150                                                                                               
    06MAY2002 140 13 3 140                                                                                               
    07MAY2002 125 19 3 125                                                                                               
    08MAY2002 127 15 3 127                                                                                               
    09MAY2002 115 12 3 115                                                                                               
    10MAY2002 156 16 3 156                                                                                               
    11MAY2002 131 12 3 131                                                                                               
    12MAY2002 156 9 3  156                                                                                               
    13MAY2002 130 16 3 130                                                                                               
    14MAY2002 106 9 3  106                                                                                               
    15MAY2002 118 14 3 118                                                                                               
    16MAY2002 116 15 3 116                                                                                               
    17MAY2002 135 18 3 135                                                                                               
    18MAY2002 117 19 3 117                                                                                               
    19MAY2002 147 15 3 147                                                                                               
    20MAY2002 152 14 3 152                                                                                               
    21MAY2002 129 23 3 129                                                                                               
    22MAY2002 115 13 3 115                                                                                               
    23MAY2002 128 20 3 128                                                                                               
    24MAY2002 118 21 3 118                                                                                               
    25MAY2002 123 9 3  123                                                                                               
    26MAY2002 130 11 3 130                                                                                               
    27MAY2002 119 12 3 119                                                                                               
    28MAY2002 130 17 3 130                                                                                               
    29MAY2002 143 18 3 143                                                                                               
    30MAY2002 136 17 3 136                                                                                               
    31MAY2002 131 17 3 131                                                                                               
    01JUN2002 143 17 3 143                                                                                               
    02JUN2002 139 19 3 139                                                                                               
    03JUN2002 139 20 3 139                                                                                               
    04JUN2002 125 17 3 125                                                                                               
    05JUN2002 107 10 3 107                                                                                               
    06JUN2002 107 10 3 107                                                                                               
    07JUN2002 125 12 3 125                                                                                               
    ;;;;                                                                                                                 
    run;quit;                                                                                                            
                                                                                                                         
    I added some structure                                                                                               
                                DAY                                                                                      
                                                                                                                         
                    7      14     21    28      35                                                                       
           --+---------+---------+---------+---------+-                                                                  
    VISITS |        |      |      |      |      |     |                                                                  
        60 +        |      |      |      |      |     +                                                                  
           |        |      *      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |        *      |      |      |      *     |                                                                  
           |        |      |      *      *      |     |                                                                  
        40 +        |      |      |     *|      |     +                                                                  
           |        |     *|      |      |      |     |                                                                  
           |        |      |      |      |      |     |                                                                  
           |       *|      |      |      |     *|     |                                                                  
           |        |      |      |    * |      |     |                                                                  
           |        |      |   * *|   *  |    * |     |                                                                  
        20 +      * |   *  |    * |      |      |     +                                                                  
           |     *  |    * |      |  *   |   *  |  *  |                                                                  
           |        |      |  *   |      |  *   |     |                                                                  
           |   **   | **   | *    |      |      | *   |                                                                  
           |        |      |      | *    | *    |     |                                                                  
           |        |      |      |      |      |     |                                                                  
         0 +  *     |*     |*     |*     |*     |*    +                                                                  
           --+---------+---------+---------+---------+-                                                                  
             0        10        20        30        40                                                                   
                                                                                                                         
                                DAY                                                                                      
                                                                                                                         
    /*           _               _                                                                                       
      ___  _   _| |_ _ __  _   _| |_                                                                                     
     / _ \| | | | __| `_ \| | | | __|                                                                                    
    | (_) | |_| | |_| |_) | |_| | |_                                                                                     
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                    
                    |_|                                                                                                  
    */                                                                                                                   
                                                                                                                         
    Next 7 Days                                                                                                          
                                                                                                                         
           39 40 41 42 43 44 45                                                                                          
         ---+--+--+--+--+--+--+----                                                                                      
    PRED |                        |                                                                                      
         |  3rd order moving      |                                                                                      
      19 +  average model?    * 19+ 19                                                                                   
      18 +                        + 18                                                                                   
      17 +  forecast next  * 17   + 17                                                                                   
      16 +  7days       * 16      + 16                                                                                   
      15 +           * 15         + 15                                                                                   
      14 +        * 14            + 14                                                                                   
      13 +                        + 13                                                                                   
      12 +     * 12               + 12                                                                                   
      11 +  * 11                  + 11                                                                                   
      10 +                        + 10                                                                                   
         |                        |                                                                                      
         ---+--+--+--+--+--+--+----                                                                                      
           39 40 41 42 43 44 45                                                                                          
                                                                                                                         
                    DAY                                                                                                  
                                                                                                                         
    Set up for a repeating 7 day period?                                                                                 
                                                                                                                         
    Forecast method: ETS(M,Ad,N)                                                                                         
                                                                                                                         
    Model Information:                                                                                                   
    ETS(M,Ad,N)                                                                                                          
                                                                                                                         
    Call:                                                                                                                
     ets(y = object, lambda = lambda, biasadj = biasadj, allow.multiplicative.trend = allow.multiplicative.trend)        
                                                                                                                         
      Smoothing parameters:                                                                                              
        alpha = 0.9174                                                                                                   
        beta  = 1e-04                                                                                                    
        phi   = 0.9691                                                                                                   
                                                                                                                         
      Initial states:                                                                                                    
        l = 1.6768                                                                                                       
        b = 4.8359                                                                                                       
                                                                                                                         
      sigma:  0.3776                                                                                                     
                                                                                                                         
         AIC     AICc      BIC                                                                                           
    284.7747 287.6712 294.2758                                                                                           
                                                                                                                         
    Error measures:                                                                                                      
                       ME     RMSE      MAE      MPE     MAPE      MASE     ACF1                                         
    Training set -2.87977 8.507852 7.021863 -34.1548 52.09824 0.9277659 0.370283                                         
                                                                                                                         
    Offset is 0                                                                                                          
                                                                                                                         
    Forecasts:                                                                                                           
       Point Forecast      Lo 80    Hi 80      Lo 95    Hi 95                                                            
    38       10.62656  5.4848833 15.76823   2.763045 18.49007   ** really 39                                             
    39       12.08717  4.3652179 19.80912   0.277465 23.89687                                                            
    40       13.50264  3.3017261 23.70354  -2.098309 29.10358                                                            
    41       14.87436  2.1622559 27.58646  -4.567125 34.31584                                                            
    42       16.20369  0.8974970 31.50988  -7.205113 39.61249                                                            
    43       17.49194 -0.5171199 35.50100 -10.050540 45.03442                                                            
    44       18.74038 -2.0964626 39.57721 -13.126818 50.60757                                                            
    >                                                                                                                    
                                                                                                                         
    /*         _       _   _                                                                                             
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                  
    / __|/ _ \| | | | | __| |/ _ \| `_ \                                                                                 
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                
                                                                                                                         
    */                                                                                                                   
                                                                                                                         
                                                                                                                         
    %utl_submit_r64('                                                                                                    
    library(forecast);                                                                                                   
    library(SASxport);                                                                                                   
    library(haven);                                                                                                      
    library(data.table);                                                                                                 
    data<-unlist(read_sas("d:/sd1/have.sas7bdat")[1:38,2]);                                                              
    mav<-ma(data[1:38], order=3);                                                                                        
    moving_average = forecast(mav, h=7);                                                                                 
    png(file="d:/png/forecast.png");                                                                                     
    plot(moving_average, ylim=c(.2,.6));                                                                                 
    want<-summary(moving_average);                                                                                       
    want<-as.data.frame(want[,1]);                                                                                       
    want<-as.data.frame(append(unlist(mav),unlist(want[,1])));                                                           
    colnames(want)="PRED";                                                                                               
    write.xport(want,file="d:/xpt/want.xpt");                                                                            
    ');                                                                                                                  
                                                                                                                         
    libname xpt xport "d:/xpt/want.xpt";                                                                                 
                                                                                                                         
    data want;                                                                                                           
      retain day 38;                                                                                                     
      format sym $2.;                                                                                                    
      set  xpt.want(firstobs=39);                                                                                        
      day=day+1;                                                                                                         
      sym=put(round(pred),2.);                                                                                           
    run;quit;                                                                                                            
                                                                                                                         
    libname xpt clear;                                                                                                   
                                                                                                                         
    proc plot data=want;                                                                                                 
      plot pred*day="*" $ sym /hpos=24 vpos=13 box vaxis=10 to 19 by 1;                                                  
    run;quit;                                                                                                            
                                                                                                                         
                                                                                                                         
                                                                                                                         
