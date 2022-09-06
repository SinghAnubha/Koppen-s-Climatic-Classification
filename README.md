import pandas as pd

#Reading data
filename = "G:/Program/Tutorial_Climate Data.xlsx"
rain_df = pd.read_excel(filename,sheet_name=0,header=[0,1,2,3],index_col=[0])
tmax_df = pd.read_excel(filename,sheet_name=1,header=[0,1,2,3],index_col=[0])
tmin_df = pd.read_excel(filename,sheet_name=2,header=[0,1,2,3],index_col=[0])

#Estimating average temoerature
Tavg_df=(tmax_df+tmin_df)/2

#Selecting a single column
daily_rain=rain_df.iloc[:,24]
daily_temp=Tavg_df.iloc[:,24]
monthwise_rain=daily_rain.resample('M').sum()
annual_sum_rain=daily_rain.resample('A').sum()

#Transforming daily data to monthly
monthly_rain=monthwise_rain.groupby(monthwise_rain.index.month).mean()
monthly_temp=daily_temp.groupby(daily_temp.index.month).mean()
annual_rain=annual_sum_rain.groupby(annual_sum_rain.index.month).mean()
annual_temp=daily_temp.groupby(daily_temp.index.month).mean()
print("monthly rainfall:",monthly_rain)
print("monthly temperature:",monthly_temp)

MAP=sum(annual_sum_rain)
Pmin=min(monthly_rain)
Pmax=max(monthly_rain)

MAT=sum(annual_temp)
Tmin=min(monthly_temp)
Tmax=max(monthly_temp)

t_mon_10=7
for i in range(24,len(monthly_temp)):
       if monthly_temp[i]>10:
           t_mon_10=t_mon_10+1

AMJJAS=[]
for i in [4,5,6,7,8,9]:
 AMJJAS.append(monthly_rain[i])
 
P_S_min=min(AMJJAS)
P_S_max=max(AMJJAS)
P_S_total=sum(AMJJAS)

ONDJFM=[]
for i in [10,11,12,1,2,3]:
    ONDJFM.append(monthly_rain[i])
    
P_W_min=min(ONDJFM)
P_W_max=max(ONDJFM)
P_W_total=sum(ONDJFM)
    
#P_threshold
if 0.7*MAP<=P_W_total:
    P_threshold=2*MAT
elif 0.7*MAP<=P_S_total:
    P_threshold=2*MAT+28
else:
    P_threshold=2*MAT+14

#classification criteria 

# Group A (Tropical)
    if Tmin>= 18:
        # Tropical Rainforest
         if Pmin>= 60:
            climate = 'Af'
            print(climate)
 # Tropical Monsoon
         elif Pmin>=100-MAP/25:
              climate = 'Am'
              print(climate)
 # Savannah with dry summer
         elif P_S_min<100-MAP/25:
              climate = 'As'
              print(climate)
 # Savannah with dry winter 
         elif P_W_min<100-MAP/25:
             climate = 'Aw'
             print(climate)
             
# Group B (Arid climate)

    elif MAP<10*P_threshold:
        #Desert 
        if MAP<5*P_threshold:
            climate = 'Bw'
            print(climate)
            
            if MAT>=18:
                climate = 'Bwh'
                print(climate)
            elif MAT<18:
                climate = 'Bwk'
                print(climate)
        elif MAP>= 5*P_threshold:
            climate = 'Bs'
            print(climate)
               
            if MAT>=18:
                climate = 'Bsh'
                print(climate)
            elif MAT<18:
                climate = 'Bsk'
                print(climate)

# Group C (temperate climate)  
           
    elif Tmax>10 & 0 <Tmin<18:
        # Dry summer
        if P_S_min<40 & P_S_min< P_W_max/3:
            climate = 'Cs'
            print(climate)
            
            # Hot summer
            if Tmax>=22:
                climate = 'Csa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Csb'
                 print(climate)
                  # Cold Summer 
            elif 1<=t_mon_10<4:
                 climate = 'Csc'
                 print(climate)
        # Dry winter
        elif P_W_min<P_S_max/10:
            climate = 'Cw'
            print(climate)
            
            # Hot summer
            if Tmax>=22:
                climate = 'Cwa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Cwb'
                 print(climate)
                  # Cold Summer 
            elif 1<=t_mon_10<4:
                 climate = 'Cwc'
                 print(climate)
                 
# Without dry season 
        else: 
            print("Not Cs or Cw")
            
               # Hot summer
            if Tmax>=22:
                climate = 'Cfa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Cfb'
                 print(climate)
                  # Cold Summer 
            elif 1<=t_mon_10<4:
                 climate = 'Cfc'
                 print(climate)
                 
# Group D (Cold/Snow Climate)    
    elif Tmax>10 & Tmin<=0:
         # Dry summer
        if P_S_min<40 & P_S_min< P_W_max/3:
            climate = 'Ds'
            print(climate)
            
            # Hot summer
            if Tmax>=22:
                climate = 'Dsa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Dsb'
                 print(climate)
                 # Very cold winter 
            elif Tmin<-38:
                    climate = 'Dsd' 
                    print(climate)
       
                 #Cold summer 
            else:
                print("Not a,b,c or d,")
                climate = 'Dsc' 
                print(climate)
                
            
             # Dry winter
        elif P_W_min<P_S_max/10:
            climate = 'Dw'
            print(climate)
        
            # Hot summer
            if Tmax>=22:
                climate = 'Dwa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Dwb'
                 print(climate)
                 
            elif Tmin<-38:
                    climate = 'Dwd'    
                
                 #Cold summer 
            else:
                print("Not a,b,c or d,")
                # Very cold winter 
                climate = 'Dwc' 
                print(climate)
         
            
             # Without dry season 
        else: 
            print("Not Ds or Dw")
            
                    # Hot summer
            if Tmax>=22:
                climate = 'Dfa'
                print(climate)
            # Warm summer
            elif t_mon_10>=4: 
                 climate = 'Dfb'
                 print(climate)
                 
            elif Tmin<-38:
                    climate = 'Dfd'    
                
                 #Cold summer 
            else:
                print("Not a,b,c or d,")
                # Very cold winter 
                climate = 'Dfc' 
                print(climate)
                
                               
 # Group E (Polar climate)
    if Tmax<10:
       climate = 'polar climate'
       print(climate)
      
       if Tmax>0:
           climate = 'Tundra'
           print(climate)
       elif Tmax<=0:
           climate = 'frost'
           print(climate)
