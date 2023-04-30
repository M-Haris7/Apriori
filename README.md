# Market Basket Analysis
Market basket analysis is a technique used in data mining and machine learning to analyze patterns of items that are frequently purchased together by customers. It is also known as association analysis or affinity analysis.

The goal of market basket analysis is to identify relationships between items that are purchased together and to use this information to improve sales and marketing strategies. For example, if a supermarket knows that customers who buy milk are also likely to buy bread, they can strategically place these items near each other in the store to encourage additional purchases.

Market basket analysis works by examining transaction data to identify sets of items that are frequently purchased together. This is done by calculating measures such as support, confidence, and lift for each itemset. The support measures the frequency of an itemset, the confidence measures the probability that an item B is purchased given that item A is purchased, and the lift measures the strength of the association between item A and item B.

Market basket analysis can be used in a variety of industries, including retail, e-commerce, and hospitality. It can help companies optimize product placement, develop targeted marketing campaigns, and improve overall customer satisfaction.

# What is Apriori algorithm?
Apriori algorithm is a popular algorithm used for market basket analysis, which is a data mining technique for discovering associations or correlations among items in a transactional database. The Apriori algorithm was introduced by R. Agrawal and R. Srikant in 1994.

The Apriori algorithm is based on the concept of frequent itemsets, which are sets of items that appear frequently together in transactions. The algorithm works by first identifying all frequent itemsets of size 1, then generating candidate itemsets of size 2 by joining frequent itemsets of size 1. The candidate itemsets are then pruned based on their support, which is the number of transactions containing the itemset. This process is repeated to generate frequent itemsets of size 2, then size 3, and so on, until no more frequent itemsets can be generated.

The Apriori algorithm uses a key measure called support, which is the proportion of transactions that contain a particular itemset. The algorithm applies a minimum support threshold, which determines the minimum frequency of occurrence required for an itemset to be considered frequent. This threshold helps to reduce the number of itemsets that need to be considered and speeds up the process of finding frequent itemsets.

The Apriori algorithm also uses the concept of association rules, which are rules that describe relationships between itemsets. Association rules have two parts: an antecedent (a set of items) and a consequent (another set of items). The confidence of an association rule is the proportion of transactions that contain the antecedent and consequent together, divided by the proportion of transactions that contain the antecedent alone. The lift of an association rule is the ratio of the observed support to the expected support if the antecedent and consequent were independent.

The Apriori algorithm is widely used in market basket analysis and other applications of data mining and machine learning. It is implemented in various programming languages and data mining software packages.

# Steps to perform Market Basket Analysis using Apriori algorithm
## Step 1- Collecting Data
### Install the csv file and copy its path
## Step 2- Exploring and preparing data
Since we're loading the transactional data, we cannot simply use the read.csv()
function used previously. Instead, arules provides a read.transactions()
function that is similar to read.csv() with the exception that it results in a sparse
matrix suitable for transactional data. The sep = "," parameter specifies that items
in the input file are separated by a comma.
## Step 3- Visualizing item support – item frequency plots
To present these statistics visually, use the itemFrequencyPlot() function. This
allows you to produce a bar chart depicting the proportion of transactions containing
certain items. Since the transactional data contains a very large number of items,
you will often need to limit the ones appearing in the plot in order to produce a
legible chart
![ML_1](https://user-images.githubusercontent.com/91845668/235347946-810728ef-ecdf-4a0a-91ef-e5455e2d69d5.png)
The histogram is then sorted by decreasing support, as shown in the following
diagram of the top 20 items in the groceries data:
![Ml_2](https://user-images.githubusercontent.com/91845668/235348007-7e996600-324f-4ff4-9df0-e50e20db8ffd.png)

## Step 4- Visualizing the transaction data – plotting the sparse matrix
In addition to looking at the items, it's also possible to visualize the entire sparse
matrix. To do so, use the image() function. The resulting diagram depicts a matrix with 5 rows and 169 columns, indicating the
5 transactions and 169 possible items we requested. Cells in the matrix are filled with
black for transactions (rows) where the item (column) was purchased.

![ML_3](https://user-images.githubusercontent.com/91845668/235348115-37be0fd3-bda4-4825-aadb-eb65bab717cb.png)

Keep in mind that this visualization will not be as useful for extremely large
transaction databases, because the cells will be too small to discern. Still, by
combining it with the sample() function, you can view the sparse matrix for
a randomly sampled set of transactions. 

![ML_4](https://user-images.githubusercontent.com/91845668/235348164-620dc96c-1db1-4a83-b498-a26d65fd4e98.png)

## Step 5- – Training a model on the data
With data preparation completed, we can now work at finding associations among
shopping cart items. We will use an implementation of the Apriori algorithm in the
arules package we've been using to explore and prepare the groceries data. You'll
need to install and load this package if you have not done so already. Although running the apriori() function is straightforward, there can sometimes
be a fair amount of trial and error needed to find the support and confidence
parameters that produce a reasonable number of association rules. If you set these
levels too high, you might find no rules or rules that are too generic to be very useful.
On the other hand, a threshold too low might result in an unwieldy number of rules,
or worse, the operation might take a very long time or run out of memory during the
learning phase.

## Step 6- Evaluating model performance
To obtain a high-level overview of the association rules, we can use summary() as
follows. The rule length distribution tells us how many rules have each count of
items. In our rule set, 150 rules have only two items, while 297 have three, and 16
have four and then we see the summary statistics of the rule quality measures: support,
confidence, and lift. The support and confidence measures should not be very
surprising, since we used these as selection criteria for the rules. We might be
alarmed if most or all of the rules had support and confidence very near the
minimum thresholds, as this would mean that we may have set the bar too high.

## Step 7- Improving model performance
Subject matter experts may be able to identify useful rules very quickly, but it would
be a poor use of their time to ask them to evaluate hundreds or thousands of rules.
Therefore, it's useful to be able to sort rules according to different criteria, and get
them out of R into a form that can be shared with marketing teams and examined in
more depth. In this way, we can improve the performance of our rules by making the
results more actionable.

### Sorting the set of association rules
Depending upon the objectives of the market basket analysis, the most useful rules
might be the ones with the highest support, confidence, or lift. The arules
package includes a sort() function that can be used to reorder the list of rules so
that the ones with the highest or lowest values of the quality measure come first.
To reorder the groceryrules object, we can apply sort() while specifying a
"support", "confidence", or "lift" value to the by parameter. By combining the
sort function with vector operators, we can obtain a specific number of interesting
rules.

### Taking subsets of association rules
Suppose that given the preceding rule, the marketing team is excited about the
possibilities of creating an advertisement to promote berries, which are now in
season. Before finalizing the campaign, however, they ask you to investigate whether
berries are often purchased with other items. To answer this question, we'll need to
find all the rules that include berries in some form.
The subset() function provides a method to search for subsets of transactions,
items, or rules. To use it to find any rules with berries appearing in the rule, use
the following command.

### Saving association rules to a file or data frame
To share the results of your market basket analysis, you can save the rules to a CSV
file with the write() function. This will produce a CSV file that can be used in most
spreadsheet programs including Microsoft Excel.
Sometimes it is also convenient to convert the rules into an R data frame. This can be
accomplished easily using the as() function.
This creates a data frame with the rules in the factor format, and numeric vectors for
support, confidence, and lift.
You might choose to do this if you want to perform additional processing on the
rules or need to export them to another database.

