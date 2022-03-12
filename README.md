#!/bin/bash



cat rateplan_report | awk '
       BEGIN{printf "PROD-BLNG-ID:" ";"; printf "BLNG-ELEM-CD:" ";"; printf "CUST-ARGT-ID:" ";"; printf "PBE-INSTC:" ";"; printf "CURRENCY-CD:" ";"; printf "DTRMNT-SET-ID:" ";"; printf "RATE-STRUC-VER-NBR:" ";"; printf "FX-DTRMNT-SET-ID:" ";"; printf "UNT-RATE:" ";"; printf "FLGFAL-AMT:" ";"; printf "NOTE:" "\n"};
         {
                        gsub(",","");
                        FS = ":";
                        if(substr($0,3,4)=="PBE:")
            {
                                printf substr($0,10,8) ";" substr($0,18,11) ";"  substr($0,44,11) ";" substr($0,68,8) ";" substr($0,86,3) ";";

            }
                        else if(substr($0,3,14)=="DTRMNT-SET-ID:")
            {
                                printf substr($0,18,8) ";" substr($0,49,3) ";";

            }
                        else if(substr($0,3,17)=="FX-DTRMNT-SET-ID:")
            {
                                printf substr($0,21,30) ";";

            }
                        else if(substr($0,2,11)=="UNT-RATE  :")
            {
                                printf substr($0,13,"\047" 29) ";";


            }
                        else if(substr($0,2,11)=="FLGFAL-AMT:")
            {
                                printf substr($0,13,29) ";";

            }
                        else if(substr($0,6,5)=="ABOVE")
            {
                                printf substr($0,6,52) "\n";

            }


        }
' > ratechange_report.csv


echo " " | mailx -s "Rate Change report Excel File" -a ratechange_report.csv  mbarbugli@gmail.com
                    
