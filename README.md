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
    git checkout MLcorrections

