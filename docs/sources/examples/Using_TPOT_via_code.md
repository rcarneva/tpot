# Using TPOT via code

We've taken care to design the TPOT interface to be as similar as possible to scikit-learn.

TPOT can be imported just like any regular Python module. To import TPOT, type:

```Python
from tpot import TPOT
```

then create an instance of TPOT as follows:

```Python
from tpot import TPOT

pipeline_optimizer = TPOT()
```

Note that you can pass several parameters to the TPOT instantiation call:

* `generations`: The number of generations to run pipeline optimization for. Must be > 0. The more generations you give TPOT to run, the longer it takes, but it's also more likely to find better pipelines.
* `population_size`: The number of pipelines in the genetic algorithm population. Must be > 0. The more pipelines in the population, the slower TPOT will run, but it's also more likely to find better pipelines.
* `mutation_rate`: The mutation rate for the genetic programming algorithm in the range [0.0, 1.0]. This tells the genetic programming algorithm how many pipelines to apply random changes to every generation. We don't recommend that you tweak this parameter unless you know what you're doing.
* `crossover_rate`: The crossover rate for the genetic programming algorithm in the range [0.0, 1.0]. This tells the genetic programming algorithm how many pipelines to "breed" every generation. We don't recommend that you tweak this parameter unless you know what you're doing.
* `random_state`: The random number generator seed for TPOT. Use this to make sure that TPOT will give you the same results each time you run it against the same data set with that seed.
* `verbosity`: How much information TPOT communicates while it's running. 0 = none, 1 = minimal, 2 = all

Some example code with custom TPOT parameters might look like:

```Python
from tpot import TPOT

pipeline_optimizer = TPOT(generations=100, random_state=42, verbosity=2)
```

Now TPOT is ready to work! You can tell TPOT to optimize a pipeline based on a data set with the `fit` function:

```Python
from tpot import TPOT

pipeline_optimizer = TPOT(generations=100, random_state=42, verbosity=2)
pipeline_optimizer.fit(training_features, training_classes)
```

then evaluate the final pipeline with the `score()` function:

```Python
from tpot import TPOT

pipeline_optimizer = TPOT(generations=100, random_state=42, verbosity=2)
pipeline_optimizer.fit(training_features, training_classes)
print(pipeline_optimizer.score(training_features, training_classes, testing_features, testing_classes))
```

Note that you need to pass the training data to the `score()` function so the pipeline re-trains the scikit-learn models on the training data.

Finally, you can tell TPOT to export the optimized pipeline to a text file with the `export()` function:

```Python
from tpot import TPOT

pipeline_optimizer = TPOT(generations=100, random_state=42, verbosity=2)
pipeline_optimizer.fit(training_features, training_classes)
print(pipeline_optimizer.score(training_features, training_classes, testing_features, testing_classes))
pipeline_optimizer.export('tpot_exported_pipeline.py')
```

Once this code finishes running, `tpot_exported_pipeline.py` will contain the Python code for the optimized pipeline.
