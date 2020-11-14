# Lab 5: Airport Histogram Data
Due: 20th November 2020

This script was created as part of Lab 5: Airport Histogram Data for IDCE 30274: Computer Programming in GIS taught by Professor Shadrock Roberts at Clark University. 

# The Code
 All data can be accessed and downloaded publicly and script can be re-run using modified file paths and names. If questions arise, users can contact Jess Strzempko at JeStrzempko@clarku.edu for more help and further information.

### What you will submit:
A link to your Github repo. The repo must contain your Python code (either as script in `.py` or a notebook in `.ipynb` format). There are four code challenges here, so be sure your code is well documented and shows us how you are solving each challenge! The `README` of your repo should briefly summarize the project (in your own words) and contain an embdded image of your final output. You do not need to include the data files you downloaded (as I have here), but can if you like. If you're going to use this repo as part of your portfolio I suggest that you include a "data" section in your readme that explains what data you've used and links to the pages where it can be downloaded (in fact, you can just copy that text from this repo!).

## Why is this lab important?
Up to now, we've been learning about the basics of Python and have done work manipulating strings, lists, dictionaries, and have started importing files such as `.txt` files. In this lab, we're going to be working with multiple files in `.csv` format. Spreadsheets are, for better or worse, the _lingua franca_ of humanitarian and international development data. As [Sarah Telford of the UN's Humanitarian Data Exchange platform tells it](https://www.devex.com/news/opinion-humanitarian-world-is-full-of-data-myths-here-are-the-most-popular-91959), "Big data is promising, but the challenge in the humanitarian sector is bringing together small amounts of non-standardized data, mostly stored in spreadsheets, from dozens of organizations to create a common picture of needs and response."

The ability to find tabular (ie. spreadsheet) data; explore it; manipulate it; and analyze it is a critical skill. Not only that, but just being able to provide basic graphical representation of quantitative information (our histogram of flight distances) to help decision makers understand the basics of data, is important. Again, to cite the article above, "...cleaning and combing data and making choices about how data will be presented... is different than drawing conclusions from the data to understand why a crisis is deteriorating, or to decide whether to prioritize a cash response rather than food distribution. Data experts create the visuals; subject matter experts do the analysis." It's important to understand that, sometimes, your role as a GIS analyst may be to prepare and display data for decision makers or subject matter experts (and [there are a LOT of them](https://blog.veritythink.com/post/60157407408/these-are-the-humanitarian-decision-makers)) for them to interpret.  

# Getting started
Before we get to the data, let's briefly touch on reading and writing comma-separated data, or data from `.csv` files. Comma-separated values is a way of expressing structured data in flat text files like this:

This is a commonly used format to get data in and out of programs like spreadsheet software, where the data are tabular. [Python comes with a CSV module](https://docs.python.org/3/library/csv.html) that provides one way to easily work with CSV-delimited data: Try downloading the `Camp_stats.csv` file, wihch is previewed above and [can be accessed in the data folder](data/Camp_stats.csv), to your desktop or local working environment. Then run the following code in Colab to choose it as a file:

Now you can open the data up in Colab! Run the following code:

Each row is read as a list of strings representing the fields in the row.

There are a few good reasons to use the CSV module here:
*   The csv module makes it clear what you’re doing to anyone reading your code.
*   The csv module is less likely to contain an error that splits some lines the wrong way.
*   The csv module has a lot of other features ([documented here](https://docs.python.org/3/library/csv.html)) that allow it to process differently formatted files, so you can easily update your program if the file format changes.

# **Reading Airport Data**
We’re going process our own data now, using freely available airline data sets from the [OpenFlights project](https://openflights.org/). Visit the [OpenFlights data page](https://openflights.org/data.html) and download their airports data file - “airports.dat”. This is a file in CSV format, open it in a text editor if you want to have a look at it. If, for some reason, you can't access the OpenFlights page or download the data, there is an [archived data set in the data folder of this tutorial](data/airports.dat).

## Challenge 1

Can you use this file to print all of the airport names for a particular country (say, Australia or Russia)? To get you started, on the OpenFlights web page it shows that “Name” is the second field in each row of data. This means in the list of fields it will have index 1 (index 0 is the first field.)

Don't forget to start by uploading the airports.dat file!

We’re going to combine everything we’ve learned into a more complex problem to solve.

OpenFlights distribute databases for both airline locations and airline route details. You can download the routes database “routes.dat” from the OpenFlights data page. This database stores every unique flight route that OpenFlights knows about. Take a moment to look at the fields available in the routes data (listed on the OpenFlights page.) Again, if for some reason, you can't access the OpenFlights page or download the data, there is an [archived data set in the data folder of this tutorial](data/routes.dat).

By using both data sources, we can calculate how far each route travels and then plot a histogram showing the distribution of distances flown.

This a multiple stage problem:
*   Read the airports file (airports.dat) and build a dictionary mapping the unique airport ID to the geographical coordinates (latitude & longitude.) This allows you to look up the location of each airport by its ID.
*   Read the routes file (routes.dat) and get the IDs of the source and destination airports. Look up the latitude and longitude based on the ID. Using those coordinates, calculate the length of the route and append it to a list of all route lengths.
*   Plot a histogram based on the route lengths, to show the distribution of different flight distances.

## Challenge 2 - Reading the airport database
Write code to read through “airports.dat” and create a dictionary mapping from an airport ID key (use the numeric ID in the first field) to the geographic coordinates. You may want to create two dictionaries, one holding latitudes and one holding longitudes.

Look back at the OpenFlights data page to see the fields available in the airports.dat file. Hint: I ended up having to look at a copy of their data in their Github repo.

## Challenge 3 - Route distances
Now that we have the lat/lon of each airport we can calculate the distance of each airline route.

Calculating geographic distances is a bit tricky because the earth is a sphere (actually, it's an oblate spheroid). The distance we measure is the “great circle distance”. We’re not going to implement our own great circle distance function in Python here, instead you can download a Python file with a `geo_distance()` function (use the file `geo_distance.py` in this repo!). Feel free to have a peek at it if you like, but don’t worry about completely understanding it at this stage. There are two ways you can use this function:

If you’re not using a Jupyter Notebook, this code snippet doesn’t display anything. You’ll need to store the result of the distance function to a variable, then add a line with a `print()` statement to display the contents of the variable.

2. As an alternative to the `import statement`, you can also copy and paste the contents of the `geo_distance.py` file into an Notebook cell. Run the cell to define the distance function, and then use it in subsequent cells!

Once you have the `distance()` function working, can you write a program that reads all the airline routes from “routes.dat”, looks up the latitude and longitude of the source and destination airports, and builds a list of route distances?

When looking at the list of fields in the OpenFlights data documentation, remember that we used the “Unique OpenFlights identifier” fields for each airport when we made the dictionaries of latitudes and longitudes, not the multi-letter airport codes.

TIP: You might come across an error like “KeyError: \N” when you first run your program. This is another problem of ‘dirty data’, the “routes.dat” file contains some airports that aren’t listed in “airports.dat”. You can skip these routes by adding a test of the type `if airport in latitudes`. Don't forget that even if you've previously uploaded the "airports.dat" file to this notebook you'll also need to add the "routes.dat" file to Colab here.

## Challenge 4 - Histogram
Now we’re ready to create a histogram displaying the frequency of flights by distance. I suggest using `plt.hist()`, which can do most of the work. You can find the [documentation on it here](https://matplotlib.org/3.3.1/api/_as_gen/matplotlib.pyplot.hist.html) and you can always look around online for more info (here's a [useful StackOverflow thread](https://stackoverflow.com/questions/33203645/how-to-plot-a-histogram-using-matplotlib-in-python-with-a-list-of-data)). The first argument you supply will be the dataset (list of distances). The second argument (try starting with 100) is the number of bins to divide the histogram into. You can increase this number to see more distinct bars and a more detailed picture, or reduce it to see a coarser picture. Try setting it to some other values and see what happens to the histogram plot. The third argument, `facecolor`, sets the color of the graph, “r” for red. There are a lot of ways to specify colors in matplotlib, all of which are [explained in the documentation](https://matplotlib.org/api/colors_api.html). All of the arguments that can be used with `hist()` can be found in the [matplotlib documentation](https://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.hist).

For my solution, I imported two libraries using `as` to "alias" them. It is possible to modify the names of modules and their functions within Python by using the `as` keyword. There are times when you may want to change a name because you have already used the same name for something else in your program, another module you have imported also uses that name, or you may want to abbreviate a longer name that you are using a lot. The construction of this statement looks like this:
