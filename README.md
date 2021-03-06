Assignment 5
============

### The problem...
One problem that confounds biologists is how best to explain the
biodiversity that surrounds us.  For example, there are antibiotic producing
and sensitive strains of the same species that reside in the same
populations.  Also, there are phage that stably co-exist with their
bacterial prey.  These types of relationships were studied in a [paper published
in the journal Nature](http://www.nature.com/nature/journal/v418/n6894/full/nature00823.html) using the model of [Rock-Paper-Scissors](http://en.wikipedia.org/wiki/Rock-paper-scissors).

### The assignment...
You will quickly notice that the authors did not make their code publicly
available and do not detail all of their assumptions. Your job for this
assignment is to attempt to recreate the line graphs shown in Figure 1 from
either of these two papers.  
At a minimum, you need to create functions that
generate the starting matrix and a 
function that uses the starting matrix to
runs the simulation for a specified number of generations.  
You also need to
make the functions flexible enough to allow the user to alter one or more set of
variables. As output, you need to generate a knitr document that describes...

* the problem
* your assumptions
* a reproduction of their line graphs
* an experiment where you outline your hypothesis and what you learned from the experiment.
* if you can figure out how to create time-lapsed heatmaps of the state of the
population (also shown in Fig. 1) as a GIF, I will give you a considerable
amount of extra credit
* you may no more than one for/repeat/while loop (although none would be even better)

### Game plan...
Complete the exercise and submit it as a pull request to the [Assignment 5 repository](https://github.com/microbialinformatics/assignment05). You should not use any packages beyond the base R system. This assignment is due on Friday, November 22nd. Be sure to include all R, Rmd, and md files in your commits. There are two approaches you and your partner can take to work on this project:

* Sit at the same computer and work together and have one person make all of the commits and pushes to GitHub.
* Divide and conquer the functions with one person creating the repository and make the other person a collaborator. Synthesize your code using pull requests


**Problem

**Assumptions

**line graphs

**Experiment
The authors use a wrap around matrix but do not explain why they do this.  It is not intuitive that bacterial "neighbors"" in a flask environment would include a bacterium that is in the farthest possible location from a given bacterium (i.e. on the other side of the flask.) We propose that the authors used this strategy to account for edge effects, such that the cells on the edge of the matrix did not have fewer neighbors than cells on the interior. We hypothesize that simulations with a matrix that does not include the wrap around feature will result in very different results after simulated competitions.  

To test this, we have removed the code for wrapping the matrix, and have run the simulation X times.  These are our results: 




```{r}
#initial set-up to start simulations
#fill matrix with randomly selected 4 choices
#C=1,S=2,R=3, E=4 empty (4 choices)
seeds<-sample(1:4,2500, replace=T) #pick the random numbers to fill matrix
initial.matrix<-matrix(seeds,nrow=50, ncol=50) #create matrix with random numbers
#pick random cell in matrix to change
####################
#function takes initial random cell(row by column, and a number of times to run) and assesses local neighborhood dynamics
local.comp<-function(initial.matrix) {
r<<-initial.matrix[(sample(1:50,1, replace=T))]
c<<-initial.matrix[(sample(1:50,1, replace=T))]
#select local neighborhood based on index
#local neighborhood is 3 above, 2 each side, 3 on bottom of index
 #rows:
#a center upper
  up.row<-r+1
  if(up.row>50){up.row<-1}
  else if(up.row<1){up.row<-50}
  # b center lower
  down.row<-r-1
  if(down.row>50){down.row<-1}
  else if(down.row<1){down.row<-50}
  #c right upper corner
  r.up.corner<-r+1
  if(r.up.corner>50){r.up.corner<-1}
  else if(r.up.corner<1){r.up.corner<-50}
  #d left upper corner
  l.up.corner<-r+1
 if(l.up.corner>50){l.up.corner<-1}
  else if(l.up.corner<1){l.up.corner<-50}
  #e center right
  center.r<-r
 if(center.r>50){center.r<-1}
  else if(center.r<1){center.r<-50}
  # f center left
  center.l<-r
 if(center.l>50){center.l<-1}
  else if(center.l<1){center.l<-50}
  #g right lower corner
  r.low.corner<-r-1
if(r.low.corner>50){r.low.corner<-1}
  else if(r.low.corner<1){r.low.corner<-50}
  #h left lower corner 
  l.low.corner<-r-1
if(l.low.corner>50){l.low.corner<-1}
  else if(l.low.corner<1){l.low.corner<-50}
   #Columns:
  #a upper
  up.col<-c
if( up.col>50){up.col<-1}
  else if( up.col<1){up.col<-50}
  # b lower
  low.col<-c
if( low.col>50){low.col<-1}
  else if( low.col<1){low.col<-50}
  # c right upper corner
  r.up.cor.col<-c+1
if( r.up.cor.col>50){r.up.cor.col<-1}
  else if( r.up.cor.col<1){r.up.cor.col<-50}
  # d left upper corner
  l.up.cor.col<-c-1
if( l.up.cor.col>50){l.up.cor.col<-1}
  else if( l.up.cor.col<1){l.up.cor.col<-50}
  # e center right
  center.r.col<-c+1
if( center.r.col>50){center.r.col<-1}
  else if( center.r.col<1){center.r.col<-50}
  # f center left
  center.l.col<-c-1
if( center.l.col>50){center.l.col<-1}
  else if( center.l.col<1){center.l.col<-50}
  # g right lower corner
  r.lower.cor.col<-c+1
if( r.lower.cor.col>50){r.lower.cor.col<-1}
  else if( r.lower.cor.col<1){r.lower.cor.col<-50}
  # h left lower corner 
  l.lower.cor.col<-c-1
if( l.lower.cor.col>50){l.lower.cor.col<-1}
  else if( l.lower.cor.col<1){l.lower.cor.col<-50}
  # put together neighbor indices
  A<<-initial.matrix[up.row,up.col]
  B<<-initial.matrix[down.row,low.col]
  C<<-initial.matrix[r.up.corner, r.up.cor.col]
  D<<-initial.matrix[l.up.corner, l.up.cor.col]
  E<<-initial.matrix[center.r, center.r.col]
  G<<-initial.matrix[center.l, center.l.col]
  H<<-initial.matrix[ r.low.corner,r.lower.cor.col]
  I<<-initial.matrix[l.low.corner, l.lower.cor.col]
#if the cell is not empty
  #change based on given probabilities:#C=1/3 C is 1
  #S0=1/4+(3/4*fractionC) s is 2
  #R is 10/32 R is 3
  #tau =3/4 and is used in delta S calculation
  #start with vector of neighbors
    make.vec<-c(A,B,C,D,E,G,H,I)
  #find how many Cs there are in neighborhood
  #calculate fraction of C to use in delta_S calculation
    ones<-length(grep(1,make.vec))
    fract.ones<-ones/length(make.vec)
#change based on the given probabilities. Cs= cell sample scheme 
  #C=1
  if(initial.matrix[r,c]==1) {newcell<-sample((c(4,1)),1,prob=(c((1/3),(1-1/3))))}
  #S=2
  else if(initial.matrix[r,c]==2) {newcell<-sample((c(4,2)),1,prob=(c(((.25+.75*fract.ones)),(1-(.25+(.75*fract.ones))))))}
  #R=3
  else if(initial.matrix[r,c]==3) {newcell<-sample((c(3,4)),1,prob=(c((1-10/32),(10/32))))}
#change the type based on what is in neighborhood if cell is empty
  #probability of it becoming 1,2,3,0 is based on probability 
  else if(initial.matrix[r,c]==4){newcell<-sample(1:8,1)} 
        if(newcell<-1){newcell<-A}
        else if(newcell<-2){newcell<-B}
        else if(newcell<-3){newcell<-C}
        else if(newcell<-4){newcell<-D}
        else if(newcell<-5){newcell<-E}
        else if(newcell<-6){newcell<-G}
        else if(newcell<-7){newcell<-H}
        else if(newcell<-8){newcell<-I}
initial.matrix[r,c]<-newcell
results<<-initial.matrix
#return matrix
   }

### replicate the sampling and changing of cells
replicate.comp<-function(initial.matrix){
  local.results<<-replicate(10,(local.comp(initial.matrix)))
  return(write.table(results, file="local.txt"))
}

###replicate sampling for global
replicate.comp.global<-function(){
  global.results<<-replicate(global.comp(),50)
  return(write.table(global.results, file="global.txt"))
  
}

  #change 
#store matrix
#run again using stored matrix...
}
 
  





```
#sources
http://stackoverflow.com/questions/9109778/random-sampling-matrix
