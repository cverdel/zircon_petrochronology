# Zircon petrochronology
This is an experiment and a work in progress to develop a zircon petrochronology archive. The purpose of this repository is to 1) archive compiled data, and 2) provide some examples of how these data can be processed and visualised using the [R language and software](https://www.r-project.org/). I am hoping to slowly add to this project as time goes on. For starters, I have added an initial version of a zircon geochronology and trace element geochemistry compilation [here](https://github.com/cverdel/zircon_petrochronology/blob/main/zircon_data_table_v0.csv). 

This figure shows where the zircons are from.
```
#df is the compiled dataset
p1 <- ggplot(df, aes(Region, ..count..)) +
  geom_bar(colour="black", fill="lightblue")+
  ylab("Number of zircons")+
  theme_bw()
p1
```
![alt text][region_plot]

[region_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/Rplot2.jpeg?raw=true


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

[age_plot]: https://github.com/cverdel/zircon_petrochronology/blob/main/Rplot_age_density.png?raw=true



