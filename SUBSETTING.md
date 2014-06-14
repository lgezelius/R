The R documentation for the subset function states "For programming it is better to use the standard subsetting functions like [, and in particular the non-standard evaluation of argument subset can have unanticipated consequences."  The following are some examples of using [ to subset R data frames, from the perspective of a someone familiar with SQL:
 
```
> # create a table (data frame)
> 
> cars <- data.frame(model=character(0), year=numeric(0), cyl=numeric(0), engine=numeric(0), 
+   stringsAsFactors = FALSE)
> 
> # insert four rows into cars (also analagous to fours unions of single row tables)
> cars <- rbind(cars, 
+   data.frame(model="Audi A4", year=2013, cyl=4, engine=2.0, stringsAsFactors = FALSE))
> cars <- rbind(cars, 
+   data.frame(model="Mercedes C250", year=2014, cyl=4, engine=1.8, stringsAsFactors = FALSE))
> cars <- rbind(cars, 
+   data.frame(model="BMW 328i xDrive", year=2013, cyl=4, engine=2.0, stringsAsFactors = FALSE))
> cars <- rbind(cars, 
+   data.frame(model="Tesla Model S", year=2013, cyl=NA, engine=NA, stringsAsFactors = FALSE))
> 
> # This is another way to load the same data
> cars <- data.frame(
+   model=c("Audi A4", "Mercedes C250", "BMW 328i xDrive", "Tesla Model S"),
+   year=c(2013, 2014, 2013, 2013),
+   cyl=c(4, 4, 4, NA),
+   engine=c(2.0, 1.8, 2.0, NA),
+   stringsAsFactors = FALSE)
> 
> # select all cars
> cars
            model year cyl engine
1         Audi A4 2013   4    2.0
2   Mercedes C250 2014   4    1.8
3 BMW 328i xDrive 2013   4    2.0
4   Tesla Model S 2013  NA     NA
>  
> # select all from cars where year = 2014
> cars[cars$year==2014, ]
          model year cyl engine
2 Mercedes C250 2014   4    1.8
> 
> # select all from cars where engine is NA
> cars[is.na(cars$engine), ]
          model year cyl engine
4 Tesla Model S 2013  NA     NA
> 
> # select model, year from cars (the result is a table aka data.frame)
> cars[, c("model", "year")]
            model year
1         Audi A4 2013
2   Mercedes C250 2014
3 BMW 328i xDrive 2013
4   Tesla Model S 2013
> 
> # select engine from cars (the result a numeric vector)
> cars[, c("engine")]
[1] 2.0 1.8 2.0  NA
> 
> # select model from cars (the result character vector)
> cars[, c("model")]
[1] "Audi A4"         "Mercedes C250"   "BMW 328i xDrive" "Tesla Model S"  
> 
> # select model, year from cars where year = 2014
> cars[cars$year==2014, c("model","year")]
          model year
2 Mercedes C250 2014
> 
> # select model, year from cars where year = 2014 and engine = 2.0
> cars[cars$year==2013&which(cars$engine==2.0), c("model","year")]
            model year
1         Audi A4 2013
3 BMW 328i xDrive 2013
4   Tesla Model S 2013
> # or an equivalent alternative is
> cars[cars$year==2013&cars$engine==2.0&!is.na(cars$engine), c("model","year")]
            model year
1         Audi A4 2013
3 BMW 328i xDrive 2013
> 
> # select model, year from cars where year = 2014 and (engine = 2.0 or engine is NA)
> cars[cars$year==2013&cars$engine==2.0, c("model", "year")]
             model year
1          Audi A4 2013
3  BMW 328i xDrive 2013
NA            <NA>   NA
> 
> # select all from cars order by model
> cars[order(cars$model), ]
            model year cyl engine
1         Audi A4 2013   4    2.0
3 BMW 328i xDrive 2013   4    2.0
2   Mercedes C250 2014   4    1.8
4   Tesla Model S 2013  NA     NA
> 
> # select model, cyl, engine from cars order by model
> cars[order(cars$model), c("model", "cyl", "engine")]
            model cyl engine
1         Audi A4   4    2.0
3 BMW 328i xDrive   4    2.0
2   Mercedes C250   4    1.8
4   Tesla Model S  NA     NA
> 
```
