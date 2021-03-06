---
layout: molecule
title: Drift Corrector
permalink: /docs/molecule/DriftCorrector/index.html
---
This command corrects all molecules for sample drift using the x_drift and y_drift information in each ImageMetaData record generated using the [DriftCalculator](../DriftCalculator). This is done in a manner such that each molecule is corrected using the drift in its ImageMetaData record.

Background region start and end points are also required. The mean of this region is subtracted from the whole trace, effectively making it approximately zero. This procedure is independent of the drift correction, which is conducted before hand for all rows in the molecule table.

The input and output columns can be changed to apply multiple corrections in sequence or correct for different kinds of background independently.

##### Inputs

* *MoleculeArchive* - MoleculeArchive for which drift will be corrected. The drift will be subtracted from the x and y columns for each molecule using the corresponding drift information in the ImageMetaData record.
* *from slice* - The first slice of the background region.
* *to slice* - The last slice of the background region.
* *MetaData Background X (x_drift)* - X column in the ImageMetaData Table to use for background correction. Usually this is x_drift.
* *MetaData Background Y (y_drift)* - Y column in the ImageMetaData Table to use for background correction. Usually this is y_drift.
* *Input X (x)* - X column to correct. Usually this is just x.
* *Input Y (y)* - Y column to correct. Usually this is just y.
* *Output X (x_drift_corr)* - The new output x column added to the DataTable with the background corrected trace. Usually this is x_drift_corr.
* *Output Y (y_drift_corr)* - The new output y column added to the DataTable with the background corrected trace. Usually this is y_drift_corr.

<img align='center' src='{{site.baseurl}}/docs/molecule/img/Drift Corrector.png' width='450' />

#### Output

* The MoleculeArchive provided as Input is modified. The drift is subtracted from the x and y columns to generate x_drift_corr and y_drift_corr. Column names will vary depending on settings...

<img align='center' src='{{site.baseurl}}/docs/molecule/img/MoleculeArchive molecule drift columns.png' width='800' />

### How to run this Command from a groovy script

```groovy
#@ MoleculeArchive archive
#@ ImageJ ij

import de.mpg.biochem.mars.molecule.*

//Make an instance of the Command you want to run...
final DriftCorrectorCommand driftCorr = new DriftCorrectorCommand();

//Populates @Parameters Services etc.. using the current context
//which we get from the ImageJ input...
driftCorr.setContext(ij.getContext())

//Set all the input parameters
driftCorr.setArchive(archive)
driftCorr.setFromSlice(100)
driftCorr.setToSlice(200)
driftCorr.setMetaX("x_drift")
driftCorr.setMetaY("y_drift")
driftCorr.setInputX("x")
driftCorr.setInputY("y")
driftCorr.setOutputX("x_drift_corr")
driftCorr.setOutputY("y_drift_corr")

//Run the Command
driftCorr.run();

//Retrieve output from the command
//In this case, the archive is just modified
//so no output is needed and the line below is not needed
//archive = driftCorr.getArchive();
```
