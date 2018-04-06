HOW TO: EVENT GENERATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Author: Jonathon Langford, Imperial College London, CMS
Description:
        The following document details how to generate events via Madgraph (v2.5.5), and subsequent showering
        using Pythia8 (v8.205). The Delphes (v3.4.1) package can then be used for reconstruction and converts 
        into standard .root format. Note, Pythia8 and Delphes have been installed separately and therefore 
        there is no need to install via the Madgraph interface.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1) INITIAL SETUP
To be performed in each new screen...
        > Go to directory: /vols/cms/heptools/EventGen/CMSSW_7_4_4/src
        > Set-up CMS environment by running command: cmsenv
        > Set-up root environment: source setup_root5.sh

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2) MADGRAPH
Package used to generate events at matrix element level (hard process)
        > Go to Madgraph directory: /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/mg5/2.5.5
        > Run Madgraph via: ./bin/mg5_aMC. 

        In mg5 interface...
                * Import required model: e.g. > import model heft
                * Specify events to be generated: e.g. > generate p p > t t~h
                * Output event config: e.g. > output test_ppTOttH
                * To generate events: > launch test_ppTOttH
                        () No need to change settings e.g. showering, as this will require installation of certain packages. All
                           showering and reconstruction done after.
                        () Change param and run cards accordingly.
        
        > Events in .lhe format in directory: /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/mg5/2.5.5/test_ppTOttH/Events/run_XX
        > Unzip: gunzip unweighted_events.lhe.gz

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3) PYTHIA8
Package used to shower events. Converts from .lhe to hepmc format.
        > Go to Pythia8 run directory: /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/pythia/pythia8205/run
        > The lheTOhepmc.cc script runs Pythia8 on events generated in Madgraph, converting from .lhe to
          hepmc. This code has already been compiled. However, if need to make changes for example adapting
          the pythia config then will need to recompile. The command to compile is in the compile_command.txt
          file.
        > Copy across .lhe event file from Madgraph directory and re-name appropriately:
          cp /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/mg5/2.5.5/test_jl_ppTOttH/Events/run_01/unweighted_events.lhe ttH_Nevents.lhe
        > Run compiled code with following input argument (file name minus .lhe): ./lheTOhepmc ttH_Nevents
        > Hepmc output in directory with following syntax: hepmc_ttH_Nevents.dat

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

4) DELPHES
Package to run reconstruction and get events in standard .root output.
        > Go to Delphes working directory: /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/delphes/Delphes-3.4.1
        > Run the DelphesHepMC executable with the following config: DelphesHepMC config_file output_file [input_file(s)]
                * config_file - configuration file in Tcl format
                * output_file - output file in ROOT format
                * input_file(s) - input file(s) in HepMC format
          e.g. ./DelphesHepMC cards/delphes_card_CMS.tcl ttH_Nevents.root /vols/cms/heptools/EventGen/CMSSW_7_4_4/src/pythia/pythia8205/run/hepmc_ttH_Nevents.dat
        > ROOT output in Delphes directory: ttH_Nevents.root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
