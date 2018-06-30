# IGR204 - Design and development of a visualization tool
How do Europeans spend their time ?

Author:
[Bolong ZHANG](https://github.com/BolongZHANG)
[Corentin ROBINEAU](https://github.com/AlwaysFurther)
[Fangda ZHU](https://github.com/zhufangda)
[Paul-Elian TABARANT](https://github.com/paulelian-tabarant)
[Timon GLASSER](https://github.com/tglasser)


## Introduction
Since the advent of computer graphics, information visualization has benefited from new possibilities of data representation. Software development kits offer many tools to the developer, allowing him to make a dynamic interface from evolving data. One of the most obvious points of interest is adding interactivity, so that the end-user can filter data according to different points of interest or specific criterias. 

Moreover, we can now run visualization interfaces directly within our browser, increasing portability. In this course, we learnt how to use D3, one of the most famous data visualization libraries for Web applications. We chose to use this tool for our project. Among the available datasets, one particularly caught our attention, “time spent”. This dataset comes from a survey lead on citizens on some European countries, and shows the amount of time dedicated on different types of activities on average, in each country. As an example, we learn that Belgian people seem to spend 11 hours and 7 minutes of their daily time for personal care on average.

The time spent can be considered in two different ways from the dataset. We can first focus on the average time spent on an activity among the total population of polled citizens in a given country. However, for some activities like studying, we may be interested in knowing the average time spent, but only on citizens who are actually students in this country. Otherwise, we will always get a zero value.   That is why we have access to a second field called “Participation time” considering only the effective activity participants. We are also provided with the associated participation rate in percents. 

If we take Greece, the average time spent on homework is 6 minutes whereas the participation time value, that is to say only on citizens actually doing homework (3.3%), is 2 hours and 57 minutes.

All data are available in a basic CSV format.


## Why designing this visualization tool ?

### Potential users

When we took this dataset, we estimated that it could serve at research purposes, especially in sociology and economy as people working in these fields are the most likely to consider large-scale phenomenons of people’s daily behaviour. This implies a certain knowledge of geographical European data but also the traditions and economic system of each country, even if not directly exploitable from our current dataset.
These researchers usually try to outline correlations between one or several factors and a particular phenomenon. With this in mind, time use in a specific country can be related to an estimated level of happiness, a repartition in money spent on different kinds of goods or issues like alcoholism. We have a wide array of applications, though we tried to focus on specific user tasks.

### Representative tasks

Within our visualization tool, the user should be able to sum up cultural practices of a given country quickly, according to a specific activity. For example, if a researcher wants to know if some European countries particularly value the old-fashioned practice of handicraft, he should be able to notice it with the least effort and time.

As participation time is intrinsically linked to the participation rate of a given activity, we considered that when the end-user focuses on participation time on a specific activity for each country, the participation rate among polled citizens should be mentioned clearly by another mean. In this way, the user will be able to have an idea of the level of dedication of each participant on the current activity (participation time), as well as the large-scale popularity or accessibility of this activity in the country (participation rate).

The last key point of our visualization is comparing relative amounts of time spent on two or more types of activities among all respondent countries. For example, some researchers may want to consider if more time is spent on personal care than on household care in Spain. It would also be useful to know if the relative values of time spent on both activities remain equivalent around Europe or rather the opposite : we might have different statements to make according to the country. The main goal is to order the activities according to their level of importance in citizens’ state of mind.

## Visualization design
### Data representation
To start thinking about potential visual representations of each key-point of our future tool, we first imagined various designs independently. From these, we kept a lot of relevant features which we basically combined to make the final design. In this part, we try to emphasize on the key-features born from this step of brainstorming.

The first aspect emerging from most of our designs proposals was making a geographical-oriented visualization. In other words, we thought that placing a map of Europe as the core element of the webpage would fit well with the aim of emitting statements on regional phenomenons beyond boundaries. We thought that it could be adapted with a color scale to put in evidence participation time and time spent, as universally known in the scholar domain.

Another key feature was the use of histograms to compare time spent on one or several activities according to each European country. Histograms are relevant to get exact values unlike the color scale mentioned above, but can also be exploited to sort countries with a criteria, the most evident being an increasing or decreasing value of time spent. The aspect of repartition plays a major role as well : some countries can show extreme values with respect to others or, otherwise, a uniform repartition of the total amount of time spent on this activity according to European countries. However, we lose the geographical dimension of data on this representation.


The most highlighted representation to compare easily the relative values of time spent on different activities was a pie chart. Although uncomfortable to read absolute values accurately, it stood as a flexible way to display relative repartition of a restricted number of activities on the map. The problem that we had to deal with on this approach was the fact that according to the size of the pie chart, some may overlap each other between different countries or even hide some little countries like Luxembourg. The type of activity could be easily defined by a colour code, for example red for ‘sleep’ and blue for ‘eating’.

A last element is missing on our representation, the relative senses of participation time and rate. As we said before, we wanted to highlight a nuance that the value of ‘time spent’, computed on the whole polled population, could not show. This nuance is the fact that two countries can have the same amount of time spent on a given activity, but according to participation time and rate, one can have a high participation time with a low participation rate, while the other has a low participation time with a high participation rate. In the first case, it shows that we have a kind of “exclusivity” on this activity in the country if it applies to the type of activity (we cannot really state this with householding for example, on which we will talk more about inequalities in tasks distribution at home). In the other case, it can be seen as a democratization of this activity in the country, which could be interesting if we focus on education as the dataset contains the activity “reading” among other things. 

To achieve highlighting these shades, the most relevant data representation that we thought about was a simple scatter plot two-dimensional graph, with participation rate along the x axis and time along the y axis (see figure below).

As we also mentioned, pie charts are good at bringing to light relative values between different activities, but we have no idea of the amount of time that the whole set of selected activities represent for a given country. With this in mind, it was also important for us to provide a way so that the user is able to compare exact values of total time spent on the selected activities, between all the countries of the dataset.

#### Types of interaction
We only talked about how data would be transposed on our application for the moment, but we also wanted to exploit the dynamic aspect of D3-based webpages to make our visualization as interactive as possible to get relevant data from it.

The key point on this topic is the way we implement a visualization containing two different quantities, time spent and participation time. As explained before, time spent is computed on the whole population while participation time is only among people who actually practice the current activity. These two aspects being totally incompatible for us and mostly depending on what the user is trying to find in the dataset, we chose to create two distinct windows to interact with each quantity. Nevertheless, we keep the same layout for both views with a geographical representation as explained before.  A simple widget drawn over the map seemed to be a good technique for us.

Another important aspect is the following : if the user chooses to look at “time spent”, how does he select the activity, or activities that he wants to visualize on the map ? To achieve that, our interface would have to offer the list of available activities and the possibility to edit our configuration (adding to, or deleting activities from the current selection). 

As we decided to combine different kinds of representation, we thought that it would also be interesting, as a second step, to provide a dynamic link between charts (scatter plots for example) and the map. For example, highlighting the point associated to a given country on the scatter plot graph when the user moves his mouse on the related country on the map would be a convenient way to move from a qualitative analysis (colours) in order to distinguish big phenomenons, to a more quantitative one in order to focus on exact values.

#### Final design
Our final design is composed with, first, a main window containing the map of Europe. We find the “toggling” widget at the top left corner of it, allowing the user to switch between the visualisation of time spent and participation time. In order to keep a certain uniformity and not to get the user lost with these two different quantities on the interface, we showed, on one side, the total amount of time spent on all the selected activities and on the other, the participation time of a given activity. In the two cases we have a colouring code, with grayscale colours for time spent and  real colours for participation time. Participation time is only drawn from a unique activity because each activity involves a single subset of the polled population. 

For time spent, the colouring code is completed by a pie chart drawn on each country, which shows the relative amount time spent on each selected activity. We first thought that we could show the total amount of time represented by selected activities by adjusting the radius of the pie chart, but as mentioned before we have the problem of overlapping pie charts. Instead, we preferred adjusting the size of the pie charts statically according to the superficy of each country, while showing the total amount of time on a histogram on the left panel.

This left panel also contains widgets to select activities from a dropdown list, associated with labels at the top, which show the currently selected activities with their relative colour of representation within the pie chart.

On the side of participation time, we retrieve the scatter plot graph mentioned before, while the dropdown list only allows to select one activity. When the user moves his mouse on a given country, we also have a tooltip displaying the participation rate.


