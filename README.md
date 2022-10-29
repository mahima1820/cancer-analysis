# cancer-analysis
This project is basically about RNA-sequencing Analysis through R programming language 
I have choosed the Cancer:- Lung Adenocarcinoma from the GEO database 
link:
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi

Steps to follow :
library(BiocManager)
library(edgeR)
library(ComplexHeatmap)
library(circlize)
library(matrixStats)
#install.packages("BiocManager")

#step 1 to read the csv file

data <- read.csv("inputfile.csv",sep=",",header=T,row.names = 1)
print(data)

#step 2 to convert in matrix
matt<- as.matrix(data)
print(matt)

#to calculate cpm

cpm1 <- apply((matt),2, function(x) x/sum(as.numeric(x)) * 10^6)
colSums(cpm1)

#to calculate the  log 

log1 <-cpm(matt,prior.count=2,log=TRUE)
#head(logCPM)[1:6,1:6]
print(log1)
summary(log1)
saveRDS(log1,file='log_data.RDS')
# to calculate the z score
library(matrixStats)

zs=(log1-rowMeans(log1))/(rowSds(as.matrix(log1)))[row(log1)]
zs[is.na(zs)]=0
zs


#calculate variance

var1<-var(zs)
var_tail<-tail(var1)
print(var_tail)


#heatmap
heatmap(var_tail,cluster_rows=T,cluster_cols=T,column_labels=colnames(z_scores),scale='row',show_rownames=FALSE)

![heat](https://user-images.githubusercontent.com/110675838/198820122-42f70b6d-82fe-4355-9f2c-67e05ca3a7eb.png)

