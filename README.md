# Data-Visualization-in-R
#Step by step process of using the ggplot2 to visualize data in R
#Datasets used: mpg and diamonds
#Introduction
install.packages("tidyverse")
library(tidyverse)
install.packages(c("nycflights13", "gapminder", "Lahman"))
library(nycflights13
library(gapminder)
library(Lahman)

#VISUALIZATION
#Let’s use our first graph to answer a question: Do cars with big engines use 
more fuel than cars with small engines? You probably already have an answer, 
but try to make your answer precise. What does the relationship between engine 
size and fuel efficiency look like? Is it positive? Negative? Linear? Nonlinear?

ggplot2::mpg
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
#same as below
ggplot(mpg, aes(displ, cty)) + geom_point()

#A graphng template
#Let’s turn this code into a reusable template for making graphs with ggplot2.
 To make a graph, replace the bracketed sections in the code below with a 
dataset, a geom function, or a collection of mappings.

ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))

ggplot(data=mpg)+ geom_point(mapping=aes(x=cyl, y=hwy))

#Aesthetic mappings
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, color=class))
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, size=class))
ggplot(data=mpg)+geom_point(mapping=aes(x=displ, y=hwy, alpha=class))
ggplot(data=mpg)+geom_point(mapping=aes(x=displ, y=hwy, shape=class))

#You can also set the aesthetic properties of your geom manually. 
For example, we can make all of the points in our plot blue:
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy), color="blue")
#What’s gone wrong with this code? Why are the points not blue?
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
	#The the color should be an argument of the goem() thus, should be outside the aes()
#Map a continuous variable to color, size, and shape. 
How do these aesthetics behave differently for categorical vs. continuous variables?
#continous
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, color=cty))
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, shape=cty))
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, size=cty))
#categorical
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, color=trans))
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, shape=trans))
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, size=trans))

#What happens if you map the same variable to multiple aesthetics?
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, color=trans, alpha=trans, size=trans))

#What does the stroke aesthetic do? What shapes does it work with? (Hint: use ?geom_point)
#Use the stroke aesthetic to modify the width of the border
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, size=cty, stroke=5))

#What happens if you map an aesthetic to something other than a variable name, 
like aes(colour = displ < 5)?
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy, color=displ<5))
# Gives the plot and returns a legend with logicals TRUE and FALSE

#FACETS
#facet by a single variable
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy))+facet_wrap(~class, nrow=2)
#facet 2 variables
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy))+facet_grid(drv~cyl)
ggplot(data=mpg)+geom_point(mapping=aes(x=displ, y=hwy))+ facet_grid(.~cyl)
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy))+facet_grid(.~class)

#What happens if you facet on a continuous variable?
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy))+facet_grid(.~cty)
	#gives a plot that doesnt make much sense
ggplot(data = mpg) + geom_point(mapping = aes(x = drv, y = cyl))
ggplot(data = mpg) + geom_point(mapping = aes(x = drv, y = cyl))+facet_grid(drv~cyl)
#What plots does the following code make? What does . do?
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +facet_grid(drv ~ .)
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +facet_grid(.~ drv)
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(. ~ cyl)
#the dot (.) facets the plot variables either horizontally or vertically.
#drv~. is tiled horizontally, .~drv is tile vertically

#What are the advantages to using faceting instead of the colour aesthetic?
 What are the disadvantages? How might the balance change if you had a larger dataset?
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy, color=class))
?facet_wrap
?facet_grid
#When using facet_grid() you should usually put the variable with more 
unique levels in the columns. Why?

#GEOMETRIC OBJECT
ggplot(data=mpg)+ geom_point(mapping=aes(x=displ, y=hwy))
ggplot(data=mpg)+geom_smooth(mapping=aes(x=displ,y=hwy))
ggplot(data=mpg)+geom_line(mapping=aes(x=displ,y=hwy))
ggplot(data=mpg)+geom_smooth(mapping=aes(x=displ,y=hwy,linetype=drv))
#Here geom_smooth() separates the cars into three lines based on their drv 
#value, which describes a car’s drivetrain. One line describes all of the 
points with a 4 value, one line describes all of the points with an f value,
 and one line describes all of the points with an r value. Here, 4 stands for
 four-wheel drive, f for front-wheel drive, and r for rear-wheel drive.

ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy, color=drv))+
	geom_smooth(mapping=aes(x=displ,y=hwy))
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy, color=drv))+
	geom_smooth(mapping=aes(x=displ,y=hwy, linetype=drv))
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy, color=drv))+
	geom_smooth(mapping=aes(x=displ,y=hwy, linetype=drv, color=drv))
?geom_smooth
	#group aesthetic
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))
ggplot(data = mpg) +geom_smooth(
    mapping = aes(x = displ, y = hwy, color = drv),
    show.legend = FALSE)
ggplot(data = mpg) +geom_smooth(
    mapping = aes(x = displ, y = hwy, color =drv))

#To display multiple geoms in the same plot, add multiple geom functions to ggplot():
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
#To avoid duplication, the above code can be writen as:
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point()+
	geom_smooth()
ggplot(data=mpg,aes(x=displ,y=hwy))+
	geom_point()+
	geom_smooth()

#If you place mappings in a geom function, ggplot2 will treat them as local
 mappings for the layer. It will use these mappings to extend or overwrite 
the global mappings for that layer only. This makes it possible to display 
different aesthetics in different layers.

ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(color=class))+
	geom_smooth()
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(color=class))+
	geom_smooth(mapping=aes(linetype=class, color=class))

#Specifying different data for different layers
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(color=class))+
	geom_smooth(data=filter(mpg,class=="subcompact"),se=FALSE)
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "pickup"), se = FALSE)
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth()
#What does the se argument to geom_smooth() do? check ?geom_smooth
#display confidence interval around smooth? (TRUE by default, see level to control
#Will these two graphs look different? Why/why not?

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()

ggplot() + 
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
#Recreate the R code necessary to generate the following graphs.

ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2))+
	geom_smooth(aes(se=FALSE))
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2))+
	geom_smooth(mapping=aes(group=drv),se=FALSE)
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2,color=drv))+
	geom_smooth(mapping=aes(group=drv, color=drv),se=FALSE)
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2,color=drv))+
	geom_smooth(aes(se=FALSE))
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2,color=drv))+
	geom_smooth(mapping=aes(linetype=drv),se=FALSE)
ggplot(data=mpg, mapping=aes(x=displ,y=hwy))+
	geom_point(mapping=aes(size=0.2, color=drv))

 #STATISTICAL TRANSFORMATIONS
head(diamonds) 
dim(diamonds)
str(diamonds)
plot(diamonds)
ggplot(data=diamonds)+geom_point(mapping=aes(x=carat,y=price,color=cut))
ggplot(data=diamonds)+geom_smooth(mapping=aes(x=carat,y=price,color=cut))

#geom_bar()
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut))
        
#You can generally use geoms and stats interchangeably. For example, 
you can recreate the previous plot using stat_count() instead of geom_bar():
ggplot(data=diamonds)+stat_count(mapping=aes(x=cut))

#POSITION ADJUSTMENT
#color vs fill aesthetics
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut, color=cut))
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut, fill=cut))
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut, fill=clarity))
	#position=identity
ggplot(data=diamonds, mapping=aes(x=cut,fill=clarity))+geom_bar(alpha=1/5,positio="identity")
ggplot(data=diamonds,mapping=aes(x=cut,color=clarity))+geom_bar(fill=NA,position="identity")
	#position=fill
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut,fill=clarity),position="fill")
	#position=dodge
ggplot(data=diamonds)+geom_bar(mapping=aes(x=cut,fill=clarity),position="dodge")
	#position=jitter
ggplot(data=mpg)+geom_point(mapping=aes(x=displ,y=hwy),position="jitter")

ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point()
#improving above plot
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point(position="jitter")

#COORDINATE SYSTEMS
ggplot(data=mpg,mapping=aes(x=class,y=hwy))+geom_boxplot()
ggplot(data=mpg,mapping=aes(x=class,y=hwy))+geom_boxplot()+coord_flip()
bar<- ggplot(data=diamonds)+
	geom_bar(mapping=aes(x=cut,fill=cut),show.legend=FALSE,width=1)+
	theme(aspect.ratio=1)+
	labs(x=NULL,y=NULL)
bar
bar+coord_flip()
bar+coord_polar()

#Turn a stacked bar chart into a pie chart using coord_polar().
ggplot(data=diamonds)+
	geom_bar(mapping=aes(x=cut,fill=clarity))
ggplot(data=diamonds)+
	geom_bar(mapping=aes(x=cut,fill=clarity))+
	coord_polar()
?labs
?geom_abline







