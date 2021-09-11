# NIBSplasticity
* package for running simulations of detailed biophysical neurons with the NEURON simulation environment as a python module.  The package is intended for simulating synaptic inputs onto pyramidal neurons during application of extracellular electric fields
* requires NEURON version 7 with python2

## Modules
	
	* run_control.py
		* used to specify parameters and procdures for specific experiments
		* see run_control.ExperimentsParallel for running parallel simulations

	* neurons.py 
		* contains methods for creating cells.  These are python objects that contain a collection of hoc sections and their inserted mechanisms

	* run.py 
		* contains methods for running simulations and saving data

	* param.py 
		* contains parameter dictionaries and methods for updating parameters for specific simulations

	* stims.py 
		* contains methods for creating stimulation objects (e.g. extracellular field, synaptic stimulation) based on parmaters specified in paramter dictionaries

	* vargen_functions.py 
		* contains methods for creating group level variables from raw neuron recordings that can then be used for analysis. 
		* e.g. if 20 cells were simulated for a given experiment (different randomizations) and their somatic membrane voltage was recorded, vargen_functions may be used to aggregrate spike data from these 20 cells into a single dataframe

	* analysis.py 
		* contains functions for post-simulation analysis

	* analysis_exp.py 
		* procedures for analyzing data from specific experiments (those specified by run_control)

## Workflow
		* to run an experiment (set of simulations)
			* call run_control.Experiment.experiment_name()
				1. set up a dictionary of parameters to be passed to the NEURON simulator (e.g. conductances) as well as experimental parameters (e.g. pattern of synaptic activation)
				2. instantiate a cell object in NEURON
				3. insert synapses and set up synaptic stimulation
				4. set up extracellular stimulation objects
				5. set up neuron recording objects
				6. run simulation
				7. save recorded data (by default saved to a subfolder of the cwd named 'Data/')

		* analysis for individual experiments
			1. generate group level variables (e.g. spikes, changes in synaptic weights, synaptic currents)
				* see modules for individual experiments with names like vargen_exp_experimentname.py
				* these modules will be used to compute synaptic plasticity with the clopath rule after running NEURON simulations.  
				* see analysis.Clopath() for code to implement the clopath rule given voltage data and input times

			2. run analyses and generate figures
				* analysis code for individual experiments (typically to generate a single figure) are contained in modules named analysis_exp_experimentname.py
				* these modules also make use of an additional module named figsetup_exp_experimentname.py which is used to specify figure parameters


