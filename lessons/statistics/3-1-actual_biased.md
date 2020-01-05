[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

>> Bruce's Answer:

>> The actual mean calculated for children per household for this data set was approximately 1.852.

>> The uppwardly baised estimate of the mean that would result from random sampling the kids is about 2.792.

>> The code used to calculate this, and to generate graphs to illustrate it, is below:

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
		List_of_Pregnancy_Outcomes = []
		List_of_Birth_Orders = []
		List_of_Number_Live_Birth = []

		while(Index < len(Raw_2002_Preg_Data)):

			String_for_Row = str(Raw_2002_Preg_Data[0][Index])
			if(String_for_Row[21] == " "):
				List_of_Number_Live_Birth.append("")
			else:
				List_of_Number_Live_Birth.append(int(String_for_Row[21]))
			List_of_IDs.append(str(String_for_Row[0:13]))
			List_of_Pregnancy_Outcomes.append(str(String_for_Row[276:277]))
			List_of_Birth_Orders.append(str(String_for_Row[277:279]))

			Index += 1

		Raw_2002_Preg_Data['Number_Live_Births'] = List_of_Number_Live_Birth

		Raw_2002_Preg_Data['Respondent_ID'] = List_of_IDs
		Raw_2002_Preg_Data['Respondent_ID'] = Raw_2002_Preg_Data.Respondent_ID.str.strip(" ")

		Raw_2002_Preg_Data['Pregnancy_Outcome'] = List_of_Pregnancy_Outcomes
		Raw_2002_Preg_Data['Pregnancy_Outcome'] = Raw_2002_Preg_Data.Pregnancy_Outcome.str.strip(" ")

		Raw_2002_Preg_Data['Birth_Order'] = List_of_Birth_Orders
		Raw_2002_Preg_Data['Birth_Order'] = Raw_2002_Preg_Data.Birth_Order.str.strip(" ")

		Raw_2002_Preg_Data.rename(columns={0: 'Raw_Data_String'}, inplace=True)

		List_of_Unique_IDs = Raw_2002_Preg_Data.Respondent_ID.unique()

		Real_Dictionary_of_Live_Births_per_Person = defaultdict(int)

		Index = 0

		while(Index < len(Raw_2002_Preg_Data)):
			if(Raw_2002_Preg_Data['Pregnancy_Outcome'][Index] == "1"):
				if(List_of_Number_Live_Birth[Index] < 6): # Higher than 5 is probably a typo
					Real_Dictionary_of_Live_Births_per_Person[Raw_2002_Preg_Data['Respondent_ID'][Index]] += List_of_Number_Live_Birth[Index]
			else:
				Real_Dictionary_of_Live_Births_per_Person[Raw_2002_Preg_Data['Respondent_ID'][Index]] += 0
			Index += 1

		Real_Distribution_of_Number_of_Kids = defaultdict(int)

		for key in Real_Dictionary_of_Live_Births_per_Person:
			Real_Distribution_of_Number_of_Kids[Real_Dictionary_of_Live_Births_per_Person[key]] += 1

		Total_Households = 0
		Total_Kids = 0

		for key in Real_Distribution_of_Number_of_Kids:
			Total_Households += Real_Distribution_of_Number_of_Kids[key]
			Total_Kids += (key * Real_Distribution_of_Number_of_Kids[key])

		Normalized_Real_Distribution_for_PMF = defaultdict(float)

		for key in Real_Distribution_of_Number_of_Kids:
			Normalized_Real_Distribution_for_PMF[key] = (Real_Distribution_of_Number_of_Kids[key]/Total_Households)

		Biased_Distribution_of_Number_of_Kids = defaultdict(int)

		for key in Real_Distribution_of_Number_of_Kids:
			Biased_Distribution_of_Number_of_Kids[key] = (key * Real_Distribution_of_Number_of_Kids[key])

		Normalized_Biased_Distribution_for_PMF = defaultdict(float)

		for key in Biased_Distribution_of_Number_of_Kids:
			Normalized_Biased_Distribution_for_PMF[key] = (Biased_Distribution_of_Number_of_Kids[key]/Total_Kids)

		plt.bar(Normalized_Real_Distribution_for_PMF.keys(), Normalized_Real_Distribution_for_PMF.values(), color = 'b')
		plt.show()
		plt.bar(Normalized_Biased_Distribution_for_PMF.keys(), Normalized_Biased_Distribution_for_PMF.values(), color = 'g')
		plt.show()

		Actual_Mean = (Total_Kids/Total_Households)

		Biased_Mean = 0

		for key in Normalized_Biased_Distribution_for_PMF:
			Biased_Mean += (key * Normalized_Biased_Distribution_for_PMF[key])

		print("The actual mean is", Actual_Mean)
		print("The biased mean that you would get from randomly asking the kids is", Biased_Mean)


