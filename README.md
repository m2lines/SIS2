# SIS2

NOAA-GFDL's Sea Ice Simulator version 2

# Disclaimer

The United States Department of Commerce (DOC) GitHub project code is provided
on an "as is" basis and the user assumes responsibility for its use. DOC has
relinquished control of the information and no longer has responsibility to
protect the integrity, confidentiality, or availability of the information. Any
claims against the Department of Commerce stemming from the use of its GitHub
project will be governed by all applicable Federal law. Any reference to
specific commercial products, processes, or services by service mark,
trademark, manufacturer, or otherwise, does not constitute or imply their
endorsement, recommendation or favoring by the Department of Commerce. The
Department of Commerce seal and logo, or the seal and logo of a DOC bureau,
shall not be used in any manner to imply endorsement of any commercial product
or activity by DOC or the United States Government.

This project code is made available through GitHub but is managed by NOAA-GFDL
at https://gitlab.gfdl.noaa.gov.

# Machine learning 

To use SIS2 with ML-based bias correction, you can compile MOM6-SIS2 in coupled mode, as outlined in the [MOM6-examples wiki](https://github.com/NOAA-GFDL/MOM6-examples/wiki/Getting-started). You will just need to make sure that when you fork MOM6-examples, you change the path to the SIS2 repo in `.gitmodules` to `https://github.com/m2lines/SIS2.git`. Then you can do:

    git clone --recursive https://github.com/yourGithub/MOM6-examples.git MOM6-examples
    cd MOM6-examples/src/SIS2
    git checkout dev/m2lines

One you compile the source code, all you then need to do is run a simulation with the correct overrides. To run in a forced ice-ocean configuration, add the following overrides in your batch script:

    touch $workDir/INPUT/SIS_override
    cat > $workDir/INPUT/SIS_override << EOF
    #override CP_SEAWATER = 3992.
    #override CP_BRINE = 3992.
    #override ICE_BULK_SALINITY = 0.0
    #override ICE_RELATIVE_SALINITY = 0.17
    #override SIS2_FILLING_FRAZIIL = T
    #override SIS_THICKNESS_ADVECTION_SCHEME = "PCM"
    #override SIS_CONTINUITY_SCHEME = "PCM"
    #override SIS_TRACER_ADVECTION_SCHEME = "PPM:H3"
    #override DO_ML = True                                                                                                                                                                                          
    #override ML_CPL = False                                                                                                                                                                                        
    #override CNN_HALO_SIZE  = 4                                                                                                                                                                        
    #override CNN_WEIGHTS = "/path/to/SIS2/ML_weights/NetworkA_weights_IO_1982-2017.nc"                                                                                        
    #override ANN_WEIGHTS = "/path/to/SIS2/ML_weights/NetworkB_weights_IO_1982-2017.nc"
    EOF

The flag `ML_CPL` will determine what normalization statistics are applied to the network inputs. If running in ice-ocean, set this to `False`, while if running in SPEAR, set this to `True` (**Unfortunately the atmosphere and land model source code for SPEAR are not made publicly available by GFDL**). You do not need to change `CNN_HALO_SIZE`. The `ML_FREQ` flag then determines how often to do inference (in seconds)---this same correction will then be applied at every timestep between inference steps. The `CNN_WEIGHTS` and `ANN_WEIGHTS` overrides are then the paths to the network weight netcdf files. These are ravelled vectors of the weights saved from PyTorch.

Tip - Besides the `SIS_ML.F90` code, look for the `!WG` flags in the other SIS2 files to understand how the ML inputs are gathered and where inference is called. 
