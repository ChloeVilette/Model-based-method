# Model-based-method
This is the code that was used to create the model-based method, which seeks to classify weak and strong ties.

  # How does the function work ?

    # What input the function needs
The function needs to be fed a vector of weigths. 
Depending on which level you are investigating strong/weak ties, you would either feed the function with weights for the overall troop, or weights for an individual.

    # Set the min_diff parameter
Once you have this weight vector, you will have to set the "min_diff" parameter depending on the length of your weight vector.
If the length of your vector is inferior or equal to 10, it is best to set min_diff to 1. If it is superior, then set it to 3.

    # What the output of the function is
Once the function ran, you will get a dataframe containing two columns : one called "weight" and the second called "strong".
The first column corresponds to your vector of weights and the second column that says whether the corresponding weight is classified as strong (1) or not (0).


The function is set to take and spit out the minimum so that you have all the flexibility you want. 
As such, if you are keen to look at partners, you may want to then attribute the corresponding weights to the right partners.
You may also add a date column if you want to look at these strong ties across time. 


In what follows, we provide a quick example as to how this function can be used. 
In this example, we will proceed to classify strong ties at the individual level. We will walk through these 5 following steps:

1. Start with dataframe with at least two columns: focal ID and partner ID.
2. From this dataframe, isolate a certain individual. 
3. From this subset, create an edgelist.
4. Feed the function with the weight column from the edgelist dataframe.
5. If keen, re-attribute partners to the corresponding weights.


# Practical example.

In this example we use a grooming dataset and we are intesrested in direction of grooming (i.e., who grooms and who receives)

    # Isolate a certain individual.
  
  subset.id <- grooming.data %>% filter (ID =="jasm")

    # Create an edgelist

  edgelist.id <- netTS::create.an.edgeList(subset.id [,c("ID", "Partner")]

    # Run the function 

  if(nrow(edgelist.id)>10){
              
    strong.ties <- model_based_strong_ties_fast(edgelist.id$weight, sd_diff = 0, min_diff=3, model_backend="rstan", normal_model_base, skewnormal_model_base, poisson_model_base, poisson_model_label)
              
      }else{
              
    strong.ties <- model_based_strong_ties_fast(edgelist.id$weight, sd_diff = 0, min_diff=1, model_backend="rstan", normal_model_base, skewnormal_model_base, poisson_model_base, poisson_model_label)
  } 

    # Re-attribute partners to the corresponding weights

  edgelist.id <- edgelist.id[order(edgelist.id$weight),]
  strong.ties <-cbind(strong.ties,edgelist.id)
  

You could obviously put this code into a loop so it repeats this process for more than one individual.
You could also add another loop to have this process repeated through time and so on...



