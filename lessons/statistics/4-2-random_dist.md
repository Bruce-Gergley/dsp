[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

>> Bruce's Answer:

>> The distribution of a random number between 0 and 1 for this problem is uniform.  Thus the probability mass function is approximately level 1 (when broken into 10 bins it has slight changes up or down for each bin on each instance of running the code) and the cumulative distribution function slopes upward from 0 to 1 with an approximately uniform slope.

>> The following code was used:

		import random
		import numpy as np
		import seaborn as sb
		import matplotlib.pyplot as plt
		from collections import defaultdict

		Number_of_Numbers = 0
		List_of_Random_Numbers = []

		Bin_Minimum_Dictionary = defaultdict(int)

		while(Number_of_Numbers < 1000):

			Random_Number = random.random()
			List_of_Random_Numbers.append(Random_Number) 

			Bin_Minimum = 0
			while(Bin_Minimum < Random_Number):
				Bin_Minimum += 0.1
			Bin_Minimum -= 0.1
			Bin_Minimum = round(Bin_Minimum, 1)
			Bin_Minimum_Dictionary[Bin_Minimum] += 1

			Number_of_Numbers += 1

		Probability_Mass_Function = sb.distplot(List_of_Random_Numbers, bins=10, kde=True, color='skyblue',
												hist_kws={"linewidth": 15,'alpha':1})
		Probability_Mass_Function.set(xlabel='Random Number Distribution', ylabel='Frequency')

		List_of_Keys = []

		for key in Bin_Minimum_Dictionary:
			List_of_Keys.append(key)

		List_of_Keys = list(sorted(List_of_Keys))

		Cumulative_Dictionary = defaultdict(float)

		Cumulative = 0

		for key in List_of_Keys:
			Cumulative += (Bin_Minimum_Dictionary[key]/Number_of_Numbers)
			Cumulative_Dictionary[key] = Cumulative

		plt.plot(Cumulative_Dictionary.keys(), Cumulative_Dictionary.values(), color = 'g')
		plt.show()
