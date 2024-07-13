# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Sasini Munasinghe

~~~

Please write your explanation here...

1. Initial Infection Sampling

infected_indices = np.random.choice(ppl.index, size=int(len(ppl) * ATTACK_RATE), replace=False)

* Sampling procedure : simple random sampling (out of 1000 people in the ppl dataframe randomly assign 100 people as infected   without replacement)
* Sample size : 1000
* Sampling frame : 1000 individuals who attended the weddings and brunches
* Underlying distribution involved: Normal distribution
* Relation to the procedure outlined in the blog post :Initial infection sampling ("Suppose that exactly 10% of people at every event are infected, regardless of the type of event)

2. Primary contact tracing sampling

ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS

* Sampling procedure : simple random sampling (100 infected people randomly assigned as traced when the randomly generated number is less than the TRACE_SUCCESS threshold of 0.2)
* Sample size : 100
* Sampling frame : 100 individuals who are infected
* Underlying distribution involved : normal distribution
* Relation to the procedure outlined in the blog post : Primary contact tracing sampling ("Suppose that contact-tracing is imperfect, and that due to faulty recall of patients and staff shortages, an infection has only a 20% chance of being traced to a source event. Call that “primary contact tracing.”" )

3. Secondary contact tracing sampling

event_trace_counts = ppl[ppl['traced'] == True]['event'].value_counts()
events_traced = event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD].index
ppl.loc[ppl['event'].isin(events_traced) & ppl['infected'], 'traced'] = True

* Sampling procedure : Cluster sampling (Count the number of traced cases in the events. Events with at least 2 primary traced cases are selected. All infected individuals at these events are then sampled)
* Sample size : 100 (All individuals who attended the events with at lest 2 primary traced cases)
* Sampling frame : 100 (All individuals who attended the events with at lest 2 primary traced cases)
* Relation to the procedure outlined in the blog post : Secondary contact tracing sampling (" If two infections are independently traced to the same source event, a special effort is made to test every person who attended that event, with the result that 100% of infections associated with that event are identified.")

~~~

![50000 simulations](../assignments/images/50000%20simulations.png)

~~~

When simulated 50000 times the data doesn't tally with original blog post graphs.In this simulation traced to weddings distribution is centered around 0.225 and according to this, contact traces are not giving a biased view of the pandemic observed cases. And the shape of the observed cases distribution is narrower than original post.

~~~

![1000 simulations](../assignments/images/1000%20simulations.png)

~~~
When 1000 simulations are done 3 times the same plot is generated every time. As the code contains random seed function (np.random.seed(10)) the same random dataset is generated every time we run the code.Hence for the same number of simulations same plot is generated.

Added more comments to the Python code for clarity. Introduced a RANDOM_SEED constant and passed random_state as a parameter to the function to ensure all random datasets are derived from the same seed. Additionally, version control systems like Git can be used to track code changes and facilitate collaboration.
~~~

## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
