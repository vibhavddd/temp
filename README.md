# temp
adding dump codes 
import time
import multiprocessing

# Define a function for classification
def classify_and_update(df, risk_type, classification_column, classifier_function):
    start_time = time.time()
    print(f"Computing {classification_column} for {risk_type}", time.strftime('%H:%M:%S', time.localtime(start_time)))
    
    # Apply the classifier_function to each row and add the result as a new column
    df[classification_column] = df.apply(lambda row: classifier_function(row), axis=1)
    
    end_time = time.time()
    print("Process Completed.", time.strftime('%H:%M:%S', time.localtime(end_time)))

# Create a pool of worker processes
pool = multiprocessing.Pool(processes=len(risktype_list))

# Define a list of tasks for each risk type
tasks = []
for risk_type in risktype_list:
    if risk_type == "Vol":
        classification_column = risk_type + 'ClassStrk'
        classifier_function = vol_strike_classifier
    else:
        classification_column = risk_type + 'ClassMaturity'
        classifier_function = classifier

    tasks.append((grip_cov.copy(), risk_type, classification_column, classifier_function))

# Perform parallel computation
results = pool.starmap(classify_and_update, tasks)

# Close the pool of worker processes
pool.close()
pool.join()

# Print completion time (optional)
end_time = time.time()
print("All Processes Completed.", time.strftime('%H:%M:%S', time.localtime(end_time)))

# Now, grip_cov contains the modifications made by the classify_and_update function

