[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> Bruce's Answer:

>> Analysis of the data indicates that first born infants weigh slightly less.

>> The Cohen Effect Size I calculated was -0.08969874168299517 (using the order ((First Born Mean) - (Not First Born Mean)) in the numerator.

>> For pregnancies with multiple live births, only the first birth birth of that pregnancy was included in this calculation.

>> The following code was used:

		import csv
		import numpy as np
		import pandas as pd
		import matplotlib.pyplot as plt
		import sys
		import thinkstats2
		import thinkplot
		from collections import defaultdict

		# Your file path may vary
		Raw_2002_Preg_Data = pd.read_csv('C:/Users/Bruce/Desktop/Metis/Coding_Bootcamp_Pre_Work/ThinkStats2/Unzipped/CSV/2002FemPreg.csv', header = None)

		Index = 0
		List_of_IDs = []
		List_of_Pregnancy_Orders = []
		List_of_Number_Live_Births = []
		List_of_Birth_Weight_Pounds = []
		List_of_Birth_Weight_Ounces = []
		List_of_Pregnancy_Outcomes = []
		List_of_Birth_Orders = []

		# For each line the data values are merged into a single string
		# The position of numbers or blank spaces in the string determine what field they are for as described in a separate file
		# This loop extracts data for the relevant fields

		while(Index < len(Raw_2002_Preg_Data)):

		    String_for_Row = str(Raw_2002_Preg_Data[0][Index])
		    List_of_IDs.append(str(String_for_Row[0:13]))
		    List_of_Pregnancy_Orders.append(str(String_for_Row[13:15]))

		    if((String_for_Row[56:58] == "  ") or (String_for_Row[58:60] == "  ")):
			List_of_Birth_Weight_Pounds.append(str(String_for_Row[56:58]))
			List_of_Birth_Weight_Ounces.append(str(String_for_Row[58:60]))
		    elif(float(int(String_for_Row[58:60]) > 15)):
			List_of_Birth_Weight_Pounds.append(float(int(String_for_Row[56:58])))
			List_of_Birth_Weight_Ounces.append(float(0))
		    else:
			List_of_Birth_Weight_Pounds.append(float(int(String_for_Row[56:58])))
			List_of_Birth_Weight_Ounces.append(float(int(String_for_Row[58:60])))

		    List_of_Pregnancy_Outcomes.append(str(String_for_Row[276:277]))
		    List_of_Birth_Orders.append(str(String_for_Row[277:279]))

		    Index += 1

		Raw_2002_Preg_Data['Respondent_ID'] = List_of_IDs
		Raw_2002_Preg_Data['Respondent_ID'] = Raw_2002_Preg_Data.Respondent_ID.str.strip(" ")

		Raw_2002_Preg_Data['Birth_Weight_Pounds'] = List_of_Birth_Weight_Pounds

		Raw_2002_Preg_Data['Birth_Weight_Ounces'] = List_of_Birth_Weight_Ounces

		# This line calculates the total weight in ounces
		Raw_2002_Preg_Data['Total_Birth_Weight_Ounces'] = ((16 * Raw_2002_Preg_Data['Birth_Weight_Pounds']) + Raw_2002_Preg_Data['Birth_Weight_Ounces'])

		Raw_2002_Preg_Data['Pregnancy_Outcome'] = List_of_Pregnancy_Outcomes
		Raw_2002_Preg_Data['Pregnancy_Outcome'] = Raw_2002_Preg_Data.Pregnancy_Outcome.str.strip(" ")

		Raw_2002_Preg_Data['Birth_Order'] = List_of_Birth_Orders
		Raw_2002_Preg_Data['Birth_Order'] = Raw_2002_Preg_Data.Birth_Order.str.strip(" ")

		Raw_2002_Preg_Data.rename(columns={0: 'Raw_Data_String'}, inplace=True)

		Live_Births_2002_Preg_Data = Raw_2002_Preg_Data[Raw_2002_Preg_Data.Pregnancy_Outcome == "1"] # Live Births Only

		#display(Live_Births_2002_Preg_Data) # If you want to look at the live birth data

		List_of_Weights_First_Borns = []
		List_of_Weights_Non_First_Borns = []
		Index = 0

		while(Index < len(Raw_2002_Preg_Data)):
		    if((Raw_2002_Preg_Data['Pregnancy_Outcome'][Index] == "1") and (type(Raw_2002_Preg_Data['Total_Birth_Weight_Ounces'][Index]) is float)):
			if(Raw_2002_Preg_Data['Total_Birth_Weight_Ounces'][Index] <= 320):
			    if(Raw_2002_Preg_Data['Birth_Order'][Index] == "1"):
				List_of_Weights_First_Borns.append(Raw_2002_Preg_Data['Total_Birth_Weight_Ounces'][Index])
			    else:
				List_of_Weights_Non_First_Borns.append(Raw_2002_Preg_Data['Total_Birth_Weight_Ounces'][Index])
		    Index += 1

		Sum_First_Born = 0
		Index = 0
		while(Index < len(List_of_Weights_First_Borns)):
		    List_of_Weights_First_Borns[Index] = (List_of_Weights_First_Borns[Index]/16) # Converts to pounds
		    Sum_First_Born += List_of_Weights_First_Borns[Index]
		    Index += 1

		Mean_First_Born = (Sum_First_Born/len(List_of_Weights_First_Borns))
		Total_Var_First_Born = 0
		Index = 0
		while(Index < len(List_of_Weights_First_Borns)):
		    Total_Var_First_Born += ((List_of_Weights_First_Borns[Index] - Mean_First_Born) ** 2)
		    Index += 1

		Variance_of_First_Born = (Total_Var_First_Born/len(List_of_Weights_First_Borns))

		Sum_Non_First_Born = 0
		Index = 0
		while(Index < len(List_of_Weights_Non_First_Borns)):
		    List_of_Weights_Non_First_Borns[Index] = (List_of_Weights_Non_First_Borns[Index]/16) # Converts to pounds
		    Sum_Non_First_Born += List_of_Weights_Non_First_Borns[Index]
		    Index += 1

		Mean_Non_First_Born = (Sum_Non_First_Born/len(List_of_Weights_Non_First_Borns))

		print("The mean wieght of first borns is", Mean_First_Born, "pounds.")

		print("The mean wieght of non first borns is", Mean_Non_First_Born, "pounds.")

		Total_Var_Non_First_Born = 0
		Index = 0
		while(Index < len(List_of_Weights_Non_First_Borns)):
		    Total_Var_Non_First_Born += ((List_of_Weights_Non_First_Borns[Index] - Mean_Non_First_Born) ** 2)
		    Index += 1

		Variance_of_Non_First_Born = (Total_Var_Non_First_Born/len(List_of_Weights_Non_First_Borns))

		Pooled_Variance = (Total_Var_First_Born + Total_Var_Non_First_Born)/(len(List_of_Weights_First_Borns) + len(List_of_Weights_Non_First_Borns))
		Pooled_Standard_Deviation = (Pooled_Variance ** 0.5)

		Cohen_Effect_Size = (Mean_First_Born - Mean_Non_First_Born)/Pooled_Standard_Deviation

		print("The Cohen Effect Size is", Cohen_Effect_Size, "(a negative value indicates first borns are lighter).")
