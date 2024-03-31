# Modelica Solver Benchmark Results

Results generated with the [modelica-solver-benchmarks system](https://github.com/lbl-srg/modelica-solver-benchmarks) are placed here for team discussion.

The purpose of these benchmarks is to keep a running record of performance and scalability of the SOEP-QSS solvers compared to traditional PyFMI solvers, particularly CVode.

## Results Notes
- Current results reflect that the real-world Building Library models have stiffnesses and derivative sensitivities that cause an excessive number of QSS time steps. Relaxation and other schemes are under development to address this.
- The log-log CPU time plots have values (not shown) at -&infin; for simulation CPU times of zero, that can occur for some of the small test models.

## Technical Notes
- While we want to assess real-world scalability, some of these models use a grid of multiple copies of the models to "scale them up". We realize that these decoupled models are not realistic scaling but they are still useful indicators of general scalability. The Scalable and OneFloor_OneZone models have more realistic scaling.
- Performance comparison faces some obstacles that the results posted here are subject to
  - QSS is very different from traditional solvers and using the same relative tolerance from QSS and CVode can yield very different solution accuracies. Tolerances can be tuned to give roughly matching accuracy and this should eventually be done for these models.
  - The total CPU time, measured from the Python driver script, is meaningful but QSS pre-simulation processing is not yet optimized for good scalability at the sizes that we are running some of these models. Looking at the provided simulation CPU time results is currently more indicative of the relative performance we expect once this processing is refactored. For now, some models show significantly worse scalability for QSS in the total CPU time than in the simulation CPU time.
- The model parameter sizes are limited for a number of these models by limits imposed by the JVM memory requirements of the FMU compiler or the size of the generated code that the C compiler can handle.
- The parameter argument used to scale the models is currently causing the `DefaultExperiment` section of the `modelDescription.xml` file to be omitted by the OCT compilation, so the `.yml` YAML configuration files need to specify the `stopTime` and `tolerance` simulation fields for each model.
