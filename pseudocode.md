 The pseudocode for the code in ACSCEND_SOFTWARE.ipynb:

1. Import necessary libraries (pandas, numpy, scipy, pickle, sklearn, mygene, time)

2. Define a function `read_rna` that takes a file path as input:
   1. Read the file as a CSV with pandas, setting the first column as the index and using tab as the separator.
   2. Keep only the first two columns of the data.
   3. Convert the index to string type.
   4. Remove any duplicate indices, keeping only the first occurrence.
   5. Remove any rows and columns with missing values.
   6. Print a message indicating the file has been loaded.
   7. Return the processed data.

3. Define a function `read_sig` that takes a file path as input:
   1. Read the file as a CSV with pandas, setting the first column as the index.
   2. Return the data.

4. Define a function `cibersort` that takes RNA data, signature data, and number of cores as input:
   1. Perform a grid search with cross-validation on a NuSVR model with specified parameters.
   2. Fit the model to the signature and RNA data.
   3. Get the best 'nu' parameter from the grid search.
   4. Fit a new NuSVR model with the best 'nu' parameter to the signature and RNA data.
   5. Get the coefficients of the model, set any negative values to 0, and normalize the weights.
   6. Return the weights.

5. Define a function `cibersort_pipeline` that takes RNA data, signature data, and number of cores as input:
   1. Find the common indices between the RNA and signature data.
   2. Filter and sort the RNA and signature data based on the common indices.
   3. Standardize the RNA and signature data.
   4. Initialize a new dataframe to store the frequencies.
   5. For each patient in the RNA data, calculate the weights using the `cibersort` function and store them in the frequency dataframe.
   6. Transpose the frequency dataframe.
   7. Print a message indicating the deconvolution is done.
   8. Return the frequency dataframe.

6. Define a function `ith` that takes cancer data as input:
   1. Apply a log2 transformation to the cancer data.
   2. Calculate the mean of each row.
   3. Calculate the z-score for each value in the cancer data.
   4. Calculate the score by taking the square root of the sum of the z-scores divided by the number of samples minus 1.
   5. Print a message indicating the inter-sample heterogeneity has been calculated.
   6. Return the score.

7. Define a function `classification` that takes a model path and RNA data as input:
   1. Load the model from the specified path.
   2. Get the feature names from the model.
   3. Remove any duplicate indices in the RNA data and transpose it.
   4. Reindex the RNA data based on the feature names, filling any missing values with 0.
   5. Make predictions with the model.
   6. Map the predictions to their corresponding classes.
   7. Print a message indicating the classification is done.
   8. Return the predictions.

8. Define a function `converter` that takes RNA data and a gene format as input:
   1. Initialize a MyGeneInfo object.
   2. If the gene format is 'Entrez' or 'Hugo', query the gene information and convert the index of the RNA data to the corresponding format.
   3. Remove any rows with 'None' as the index.
   4. If the gene format is not 'Entrez' or 'Hugo' or 'Ensembl', print a message asking for an appropriate gene format.
   5. Return the RNA data.

9. Define a function `final_pipe` that takes paths to RNA data, signature data, and a model, and a gene format as input:
   1. Read the RNA and signature data using the `read_rna` and `read_sig` functions.
   2. Convert the RNA data to the specified gene format using the `converter` function.
   3. Perform deconvolution on the RNA and signature data using the `cibersort_pipeline` function.
   4. Calculate the inter-sample heterogeneity of the RNA data using the `ith` function.
   5. Classify the RNA data using the `classification` function.
   6. Prepare the final data by setting the columns to the last row, removing the last row, and inserting the class and ITH as the first two columns.
   7. Return the final data.

10. Define paths to the RNA data, signature data, and model.

12. Print the final data of the function.