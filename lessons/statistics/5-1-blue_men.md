[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

	Bruce's Answer:

	About 34.27 percent of the male population is in the eligible height range.
	
	This can be calculated by calculating the number of standard deviations above or below the mean for the minimum and maximum 
	heights, and then calculating the area under the curve, as shown by the Python code below:
	
		import numpy as np
		import pandas as pd
		import matplotlib.pyplot as plt
		import sys
		import thinkstats2
		import thinkplot
		from collections import defaultdict
		from scipy.stats import norm

		Minimum_Height_In_Inches = 70 # Source: Chapter 5 Ex 1
		Maximum_Height_In_Inches = 73 # Source: Chapter 5 Ex 1

		Centimeters_Per_Inch = 2.54 # Source: Google Search

		Minimum_Height_In_Centimeters = (Centimeters_Per_Inch * Minimum_Height_In_Inches)
		Maximum_Height_In_Centimeters = (Centimeters_Per_Inch * Maximum_Height_In_Inches)

		Mean_Centimeters = 178 # Source: Chapter 5 Ex 1
		Standard_Deviation_Centimeters = 7.7 # Source: Chapter 5 Ex 1

		Minimum_Standard_Deviations_Above_Mean = ((Minimum_Height_In_Centimeters - Mean_Centimeters)/Standard_Deviation_Centimeters)
		Maximum_Standard_Deviations_Above_Mean = ((Maximum_Height_In_Centimeters - Mean_Centimeters)/Standard_Deviation_Centimeters)

		Proportion_Between_Minimum_and_Maximum = (norm.cdf(Maximum_Standard_Deviations_Above_Mean) - norm.cdf(Minimum_Standard_Deviations_Above_Mean))

		print(Proportion_Between_Minimum_and_Maximum)

		Percent_Eligible = (100 * round(Proportion_Between_Minimum_and_Maximum, 4))

		print("About", Percent_Eligible, "percent of the male population is of eligible height for the Blue Man group.")
