# Master on Health Molecular Technologies 

## Exercises - Tutorial R


 The purpose of this tutorial is to familiarize you with the R computing environment.





**Exercise 1**: What is (1198/2) - 63 + 27?


<details><summary>Click Here to see the answer</summary><p>

```{r}

1198/2-63+27

```

</p></details>

<br/>

**Exercise 2**: Set _a_ equal to 7 and _b_ equal to 17 and _c_ equal to their product.

<details><summary>Click Here to see the answer</summary><p>

```{r}
a<-7
b<-17
c<-a*b
c

```

</p></details>

<br/>


**Exercise 3**: Type the following into R:



```{r}
xx <- c(29,3,6,11,0,41,101)

```


<br/>

**Exercise 4**: Print out the 4th element of xx.

<details><summary>Click Here to see the answer</summary><p>

```{r}
xx[4]

```

</p></details>

<br/>

**Exercise 5**: What is the length of xx?

<details><summary>Click Here to see the answer</summary><p>

```{r}
length(xx)

```

</p></details>

<br/>




**Exercise 6**: Print out the 4th and 7th elements of xx.

<details><summary>Click Here to see the answer</summary><p>

```{r}
xx[4]

```

</p></details>

<br/>


**Exercise 7**: Look up in help the following functions: _sd()_, _min()_, _max()_ and then use these functions on the vector xx.


<details><summary>Click Here to see the answer</summary><p>

```{r}
?sd
?min()
?max()

sd(xx)
min(xx)
max(xx)

```

</p></details>

<br/>



**Exercise 8**: Set the vector "xvals" equal to the numbers from -2.5 to +2.5 in increments of .02. How many elements are in this vector?

<details><summary>Click Here to see the answer</summary><p>

```{r}
xvals<-seq(-2.5,2.5,0.02)
length(xvals)

```

</p></details>

<br/>
 
 
 
**Exercise 9**: Type in the following command to R:
 
```{r}
mat <- matrix(seq(21,71,10),nrow=3)

```

**Exercise 10**: Print out the value in the 3nd row, 2nd column of mat.


<details><summary>Click Here to see the answer</summary><p>

```{r}
mat[3,2]

```

</p></details>

<br/>


**Exercise 11**: Print out the last row.


<details><summary>Click Here to see the answer</summary><p>

```{r}
mat[3,]

```

</p></details>

<br/>


**Exercise 12**: Print out the 2nd column.

<details><summary>Click Here to see the answer</summary><p>

```{r}
mat[,2]

```

</p></details>

<br/>





**Exercise 13**: Attach the data CO2.

<details><summary>Click Here to see the answer</summary><p>

```{r}
attach(CO2)

```

</p></details>

<br/>




**Exercise 14**: How many variables are in the data?



<details><summary>Click Here to see the answer</summary><p>

```{r}
names(CO2)
length(CO2)

```

</p></details>

<br/>


**Exercise 15**: How many cases are in the data?

<details><summary>Click Here to see the answer</summary><p>

```{r}
dim(CO2)

```

</p></details>

<br/>





**Exercise 16**: Get the _summary_ function for the variable "uptake".

<details><summary>Click Here to see the answer</summary><p>

```{r}
summary(CO2$uptake)

```

</p></details>

<br/>



**Exercise 17**:The _read.table_ function loads data from a file (either from the web or from your hard drive) and returns a data frame variable. Get the data
from the website http://www-stat.stanford.edu/rag/stat141/exs/MAO.dat


<details><summary>Click Here to see the answer</summary><p>

```{r}
MAO<-read.table("http://www-stat.stanford.edu/~rag/stat141/exs/MAO.dat", header=TRUE)

```

</p></details>

<br/>




**Exercise 18**: Access the variables of the dataframe.


<details><summary>Click Here to see the answer</summary><p>

```{r}
names(MAO)

```

</p></details>

<br/>


**Exercise 19**: Save the dataframe on your working directory.

<details><summary>Click Here to see the answer</summary><p>

```{r}
save(file="mao.Rdata")

```

</p></details>

<br/>


**Exercise 20**: The _tapply_ function allows you to execute a function repeatedly on different groups. Get the mean of the variable "MAO.activity" of the entering students from each group.

<details><summary>Click Here to see the answer</summary><p>

```{r}
attach(MAO)
tapply(MAO.activity,group,mean)

```

</p></details>

<br/>



**Exercise 21**: Produce an histogram for the variable "MAO.activity". 

<details><summary>Click Here to see the answer</summary><p>

```{r}
hist(MAO.activity,col="red",main="MAO.activity")

```

</p></details>

<br/>


**Exercise 22**: Produce a pie graph to the variable "group".


<details><summary>Click Here to see the answer</summary><p>

```{r}
detach(MAO)
pie(MAO$group)

```

</p></details>

<br/>


##Glossary of some terms


_command-line environment_ - A computer environment in which you type com-
mands at a prompt, and then the computer executes them. Examples are the
DOS C: environment, the UNIX shells, and the R, MATLAB, and Python
programming environments.


_script_ - A list of commands saved in a le that you can execute sequentially. In R, go to File:Open script.

_function_ - A command that takes zero or more arguments as input and can
return a number, vector, data frame, etc. as output. The output is called
the return value.


_variable_ -A named number, vector, data frame, etc. that R stores in its memory. A variable can be modified.

_data frame_. A variable that is in the form of a table of data.




