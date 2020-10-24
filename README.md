# Zircon petrochronology
This is an experiment and a work in progress to develop a zircon petrochronology archive. The purpose of this repository is to 1) archive compiled data, and 2) provide some examples of how these data can be processed and visualised using the [R language and software](https://www.r-project.org/). I am hoping to slowly add to this project as time goes on. Motivation for this comes mainly from various papers I've read from [George Gehrels](https://www.geo.arizona.edu/Gehrels), [Brenhin Keller](https://brenhinkeller.github.io/), [Christopher Spencer] (http://tectonochemistry.com/about/), and [Pieter Vermeesch}(https://www.ucl.ac.uk/earth-sciences/people/academic/prof-pieter-vermeesch).

For starters, I have added an initial version of a zircon geochronology and trace element geochemistry compilation [here](https://github.com/cverdel/zircon_petrochronology/blob/main/zircon_data_table_v0.csv). 

This figure shows where the zircons are from.
```
#df is the compiled dataset, something like df<-read_csv("zircon_data_table_v0.csv")
p1 <- ggplot(df, aes(Region, ..count..)) +
  geom_bar(colour="black", fill="lightblue")+
  ylab("Number of zircons")+
  theme_bw()
p1
```
![alt text][region_plot]

[region_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/figures/Rplot2.jpeg?raw=true


Below is a density plot of the ages of zircons in the compilation.
```
p2<-ggplot(df, aes(x=Selected_age)) + 
  geom_density(alpha=.9, fill="lightblue")+
  xlab("Age (Ma)")+
  theme_bw()+
  theme(axis.title.y=element_blank(),
        axis.text.y=element_blank(),
        axis.ticks.y=element_blank())
p2
````
![alt text][age_plot]

[age_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/figures/Rplot_age_density.jpeg?raw=true

Th and U concentrations are reported in nearly every zircon geochronology study, and Th/U is frequently used (for better or worse) to distinguish "igneous" from "metamorphic" zircons (the rationale being that zircons that compete for Th with monazite during metamorphic proesses will have lower Th/U than their "igneous" zircon counterparts). It could be interesting therefore to plot Th/U vs. age for zircons in the dataset. In the example below this is done with boxplots in an effort to illustrate the most common zircon Th/U in different age bins (here chosen as 50 My). 
```
df$ThU<-df$Th/df$U #Calculates Th/U

#Sets some parameters
lwd=.005 #linewidth of boxplots
interval=50 #bin interval
oa=0.05 #transparency of outlying points

#Creates plot
p3<-ggplot(df, aes(x=Selected_age, y=ThU))+
  geom_boxplot(outlier.alpha=oa, lwd=lwd, fill='lightblue', aes(group=cut_width(Selected_age, interval, boundary = 0)))+
  scale_y_log10()+
  geom_vline(xintercept=c(541, 1000, 1600, 2500, 4000))+
  xlab("Age (Ma)")+
  ylab("Th/U")+
  theme_bw()
p3
````
![alt text][ThU_plot]

[ThU_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/figures/Rplot_ThU_boxplot.jpeg?raw=true

It could be useful to view these data divided by the general regions in which they were collected. Below the data are shown as 2D density plots instead of boxplots.
```
p4<-ggplot(df, aes(x=Selected_age, y=ThU))+
  geom_vline(xintercept=c(541, 1000, 1600, 2500, 4000))+
  scale_fill_gradientn (colours=(blue2red(100))) +
  stat_density2d(aes(fill=..level..,alpha=..level..),bins=40, geom='polygon', h = c(250,1), show.legend = F) + 
  geom_point(size=.5, alpha=0.02)+
  scale_y_log10(limits=c(.02,10))+
  scale_x_continuous(limits=c(0, 4500), expand = c(0, 0)) +
  xlab("Age (Ma)")+
  ylab("Th/U")+
  theme_bw()+
  facet_wrap(~Region, ncol=2)
p4
````
![alt text][ThU_faceted_plot]

[ThU_faceted_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/figures/faceted_ThU.jpeg?raw=true

Ti concentration is frequently used for [zircon thermometry](http://homepages.rpi.edu/home/61/watsoe/yesterday/public_html/research/Watson_etal_CMP06.pdf). The following plot shows Ti concentration vs Th/U for the entire dataset. The point in the middle of the graph shows the median Ti concentration and Th/U of zircon.
```
#Determines median Th/U and Ti concentration
medThU<-as.numeric(format(round(median(df$ThU, na.rm = TRUE),1)))
medTi<-as.numeric(format(round(median(df$Ti, na.rm=TRUE),2)))
coords = paste(medTi,medThU,sep=",")

#Creates plot
p5<-ggplot(df, aes(x=Ti, y=ThU))+
  scale_fill_gradientn (colours=(blue2red(100))) +
  stat_density2d(aes(fill=..level..,alpha=..level..),bins=40, geom='polygon', h = c(1,.1), show.legend = F) + 
  geom_point(size=.5, alpha=0.1)+
  geom_point(aes(x=medTi, y=medThU), size=3, colour="white")+
  annotate("text",x=medTi, y=medThU+0.15, label=coords, colour="white")+
  scale_y_log10(limits=c(.02,10))+
  scale_x_log10(labels=scales::number_format(accuracy = 0.1))+
  xlab("Ti (ppm)")+
  ylab("Th/U")+
  theme_bw()
p5
````
![alt text][ThU_Ti_plot]

[ThU_Ti_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/figures/ThU_Ti_plot.jpeg?raw=true


