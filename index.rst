:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **This technote is a work-in-progress.**

Abstract
========

This technote reports the data analysis M2 no-back driving for the axial and tangent actuators. The tests have been conducted at the Level 3 
and the test case formulation and execution can be found here:

- Axial actuators: https://jira.lsstcorp.org/secure/Tests.jspa#/testCase/LVV-T2873
- Tangent actuators: https://jira.lsstcorp.org/secure/Tests.jspa#/testCase/LVV-T1784

The Jupiter Notebook scripts of the data analysis can be found here:

TBW ADD GitHub links

The Jupiter Notebook script used for the test execution on the axial actuators can be found here: 
https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m2/m2_functional_verification/LVV-T1784-no_back_driving.ipynb
The tangent actuators do not require any test script since the M2 surrogate wieght is used as an equivalent applied force close to the closed-loop maximum force limit. 


Requirements to be verified
===========================


.. _LTS-146-REQ-0084-V-02 : https://jira.lsstcorp.org/browse/LVV-18720 
.. _LTS-146-REQ-0084-V-03 : https://jira.lsstcorp.org/browse/LVV-18723 


`LTS-146-REQ-0084-V-02`_ 3.3.1.4 GRAVITATIONAL ORIENTATION OFF-TELESCOPE - M2 LSST Re-verification
`LTS-146-REQ-0084-V-03`_ 3.3.1.8 NO BACK-DRIVING - M2 LSST Re-verification





Axial actuators
================

The no-back driving behaviour of the axial actuators has been tested in two configurations: with the M2 surrogate facing downward and the M2 weight pulling the actuators, 
and with the M2 surrogate facing upward with the M2 weight pushing the actuators.
The axial actuators have been tested subdividing the 72 actuators in 8 groups of 12 actuators each. Within each group to avoid 
excessive stress on the actuators, half of them (6 units) have been pushed towards the positive force limit value (444N) 
and the other half was pushed towards the negative force limit value (-444N) as shown in :ref:`Figure 1 <fig1>`. The force to be applied is estimated by retrieving a statistics
of the axial actuator measured forces from the :abbr:`EFD (Engineering Facility Database)` and their oscillations as follows:

.. math:: 
    F_{applied} = 430 - (F_{measured} + PtV_{\Delta F})


The test script deliberately keeps a contingeny (430N instead of 444N) to avoid triggering any faults for maximum force limits in closed-loop.

.. image:: /_static/72_array.png
   :target: ../_images/72_array.png
   :alt: LSST Logo
.. _fig1:

   :ref:`Figure 1 <fig1>`. Map of the 72 axial actuators. Of each tested group made of 12 actuators, half of them (large yellow circles) are pushed towards the positive force limit value (444N) 
   and the other half (small blue circles) towards the negative force limit value (-444N) which is not achieved in absolute value because all the actuators have non-zero force controlled 
   by the force balance to distribute the load of the M2 surrogate weight.


Once the applied force is commanded the force is held for approximately 30 sec before cutting the M2 cell power by pushing the Estop button. This window is needed to acquire a reliable
measurement of the measured forces and :abbr:`RBP (Rigid Body Position)` as measured by the :abbr:`IMS (Independent Measurement System)` before the power cut to compare with the same
quantities following the power cut. This comparison is the core of the no-back driving test. :ref:`Figure 2 <fig2>` shows the drops of the
motor current following the push of the Estop.


.. image:: /_static/estop.png
   :target: ../_images/estop.png
   :alt: LSST Logo
.. _fig2:

   :ref:`Figure 2 <fig2>`. The motor current of the M2 cell drops to zero following the E-stop push.

The typical profile of the measured forces before and after the commanded force (orange) and the Estop push (green) is reported in :ref:`Figure 3 <fig3>`. 

.. image:: /_static/measured_and_times.png
   :target: ../_images/measured_and_times.png
   :alt: LSST Logo
.. _fig3:

   :ref:`Figure 3 <fig3>`. The profile of the measured forces from 12 axial actuators with the highlighted times of the commanded applied force (orange) and the Estop push (green).
 

The measured forces from 12 axial actuators before (blue) and after (orange) the Estop push are shown in :ref:`Figure 4 <fig4>`. 
 
 .. image:: /_static/Measured_forces.png
    :target: ../_images/Measured_forces.png
    :alt: LSST Logo
 .. _fig4:

    :ref:`Figure 4 <fig4>`. Profiles of the measured forces from 12 axial actuators before (blue) and after (orange) the Estop push.
     

To get a reliable proof of the no-back driving behaviour of the axial actuators we look at the motor encoder position after the Estop push. 
A null angular coefficient of the linear fit of the motor encoder position proves that the actuators do not back drive and verifies
the requirement `LTS-146-REQ-0084-V-03`_ 3.3.1.8 NO BACK-DRIVING - M2 LSST Re-verification. The typical motor encoder position and
fit are shown in :ref:`Figure 5 <fig5>`.

 .. image:: /_static/encoder_fit.png
    :target: ../_images/encoder_fit.png
    :alt: LSST Logo
 .. _fig5:

    :ref:`Figure 5 <fig5>`. Linear fit (dashed line) of the motor encoder position (yellow) from 12 axial actuators after the Estop push.
     
     



.. _table-label2:
.. table:: Fit of the axial actuator encoder values (steps/sec) after E-stop push, M2 facing down

    ========   ========   ========   ========    ========   ========    ========  ========
    GROUP 1    GROUP 2    GROUP 3    GROUP 4     GROUP 5    GROUP 6     GROUP 7   GROUP 8
    ========   ========   ========   ========    ========   ========    ========  ========
    2.3e-16    -8.2e-16   -9.3e-16   -4.4e-16    5.4e-17    2.7e-16     5.5e-06   3.3e-17
    -1.4e-16   1.2e-16    7.7e-07    -6.9e-17    -6.7e-18   1.4e-16     -2.0e-16  -1.7e-17
    5.8e-08    8.7e-17    -3.6e-18   -5.5e-17    1.7e-18    3.8e-16     -5.7e-16  2.6e-16  
    3.5e-16    -9.8e-21   -8.7e-17   -1.9e-06    -6.7e-18   -6.8e-17    6.5e-17   -4.1e-17  
    7.3e-18    -1.9e-20   4.6e-07    8.8e-07     -1.3e-17   9.5e-17     -4.3e-17  -4.9e-17 
    -1.3e-16   9.8e-21    -5.8e-16   -6.1e-16    1.1e-16    8.7e-16     -3.4e-16  -6.6e-17 
    5.8e-16    -1.8e-16   -5.8e-16   -2.8e-16    1.1e-16    3.0e-16     -5.7e-17  9.9e-17 
    3.5e-16    -4.4e-17   -1.7e-16   -2.2e-16    1.3e-16    3.4e-17     1.4e-17   2.1e-18  
    -8.0e-16   -3.6e-18   2.5e-17    -1.4e-17    2.2e-16    4.3e-16     -3.4e-16  2.0e-16 
    2.9e-16    1.2e-16    -5.8e-16   -5.5e-17    2.7e-17    1.1e-16     2.8e-06   6.6e-17  
    5.8e-17    -6.6e-17   -8.7e-17   -1.7e-16    2.7e-17    2.7e-17     1.8e-18   5.2e-19
    7.9e-19    5.8e-17    -1.7e-16   -3.0e-16    5.4e-17    5.9e-16     -4.6e-16  -6.6e-17
    ========   ========   ========   ========    ========   ========    ========  ========
    
    
    
    
     
.. _table-label3:
.. table:: Fit of the axial actuator encoder values (steps/sec) after E-stop push, M2 facing up

     ========  ========  ========  ========  ========   ========    ========  ========
     GROUP 1   GROUP 2   GROUP 3   GROUP 4   GROUP 5    GROUP 6     GROUP 7   GROUP 8
     ========  ========  ========  ========  ========   ========    ========  ========
     1.3e-16   0         -7.2e-17  2.4e-17   -1.7e-16   -1.5e-16    -4.3e-16  5e-16 
     -3.4e-06  6.4e-17   -2.5e-06  3.2e-17   -8.9e-18   -2.6e-17    -1.6e-16  2.6e-17 
     -2e-16    -7.7e-17  -6.8e-07  1.5e-07   0          -9.2e-06    -2.4e-16  2.1e-16
     -7e-07    3.1e-21   -5.7e-17  7e-06     -8.5e-08   -3.1e-16    -3.2e-16  1.5e-16
     -5.7e-17  6.3e-21   -1.7e-16  1.1e-16   2.8e-17    -5.1e-17    -2.1e-16  6.2e-17 
     -8.5e-17  -3.1e-21  -2.9e-17  4.8e-17   4.2e-17    0           1.3e-17   7.3e-17
     -2.3e-16  9.7e-18   -5.7e-17  8e-17     -2e-16     2e-16       -2.1e-16  1.7e-16
     -2.8e-16  5.2e-17   -3.4e-16  -3.1e-07  -1.4e-16   -5.1e-17    -2e-17    1.2e-16
     -1.7e-16  1.2e-16   -8.6e-17  4.8e-17   -4.2e-07   -5.1e-17    -3.3e-17  -1e-16 
     -4.3e-17  1e-16     -3.6e-18  5.6e-17   0          -1e-16      -2.9e-07  -2.3e-17
     -1.7e-16  5.2e-17   0         1.6e-16   -1.7e-16   -5.1e-17    1.3e-17   5e-06
     -2.1e-06  2.1e-16   -1.4e-16  -1.6e-17  2.8e-17    2.6e-17     -2.4e-06  -6.2e-17 
     ========  ========  ========  ========  ========   ========    ========  ========  
    
    
During the power cut the :abbr:`RBP (Rigid Body Position)` is monitored using the :abbr:`IMS (Independent Measurement System)` to verify that its position
does not change when the M2 cell is unpowered. See :ref:`Figure 6 <fig6>` for the trend of the 6 degrees of freedom.


 .. image:: /_static/IMS.png
    :target: ../_images/IMS.png
    :alt: LSST Logo
 .. _fig6:

    :ref:`Figure 6 <fig6>`. Behaviour of the 6 degrees of freedom of the M2 surrogate :abbr:`RBP (Rigid Body Position)` as monitored by the :abbr:`IMS (Independent Measurement System)`.

This check verifies the requirement `LTS-146-REQ-0084-V-02`_ 3.3.1.4 GRAVITATIONAL ORIENTATION OFF-TELESCOPE - M2 LSST Re-verification by
proving that the mirror support system safely supports the M2 mirror for any orientation of the M2 cell relative to the gravity vector, from 0 degrees (zenith) to 90 degrees (horizon),
while the M2 Cell Assembly is powered on or off.

    

.. _table-label:
.. table:: :abbr:`RBP (Rigid Body Position)` measured by the :abbr:`IMS (Independent Measurement System)` at the force command time and following the eStop push.

    ======= ==============  ================
    IMS DoF Command Time    eStop
    ======= ==============  ================
    GROUP1  Command Time    eStop
    x       -5.57 +/- 0.02  -5.6 +/- 0.00
    y       4.19 +/- 0.04   4.2 +/- 0.00
    z       10.58 +/- 0.01  1.1e+01 +/- 0.10
    xRot    4.88 +/- 0.00   4.9 +/- 0.01
    yRot    1.60 +/- 0.00   1.6 +/- 0.01
    zRot    0.44 +/- 0.00   0.44 +/- 0.00
    GROUP2  Command Time    eStop
    x       -3.70 +/- 0.07  -3.7 +/- 0.00
    y       1.74 +/- 0.05   1.7 +/- 0.00 
    z       29.47 +/- 0.06  2.9e+01 +/- 0.03
    xRot    15.88 +/- 0.01  1.6e+01 +/- 0.00
    yRot    2.87 +/- 0.01   2.9 +/- 0.00
    zRot    0.28 +/- 0.01   0.27 +/- 0.00
    GROUP3  Command Time    eStop
    x       -7.42 +/- 0.00  -7.4 +/- 0.00
    y       3.03 +/- 0.00   3.0 +/- 0.00
    z       6.18 +/- 0.03   6.2 +/- 0.01
    xRot    2.41 +/- 0.00   2.4 +/- 0.00
    yRot    4.68 +/- 0.01   4.7 +/- 0.00
    zRot    0.41 +/- 0.00   0.41 +/- 0.00
    GROUP4  Command Time    eStop
    x       -6.84 +/- 0.04  -7.0 +/- 0.00
    y       2.31 +/- 0.00   2.3 +/- 0.00
    z       5.25 +/- 0.01   5.2 +/- 0.00
    xRot    0.80 +/- 0.01   0.83 +/- 0.00
    yRot    3.59 +/- 0.02   3.5 +/- 0.00
    zRot    0.39 +/- 0.00   0.38 +/- 0.00
    GROUP5  Command Time    eStop
    x       -7.92 +/- 0.00  -7.9 +/- 0.01
    y       1.88 +/- 0.00   1.9 +/- 0.02
    z       3.48 +/- 0.04   3.5 +/- 0.02
    xRot    -0.67 +/- 0.01  -0.68 +/- 0.00
    yRot    2.81 +/- 0.00   2.8 +/- 0.00
    zRot    0.43 +/- 0.00   0.43 +/- 0.00
    GROUP6  Command Time    eStop
    x       -7.15 +/- 0.08  -7.2 +/- 0.03
    y       2.17 +/- 0.01   2.2 +/- 0.05
    z       3.24 +/- 0.03   3.2 +/- 0.00
    xRot    -1.70 +/- 0.00  -1.7 +/- 0.00
    yRot    0.47 +/- 0.01   0.47 +/- 0.00
    zRot    0.46 +/- 0.00   0.45 +/- 0.00
    GROUP7  Command Time    eStop
    x       -6.67 +/- 0.00  -6.7 +/- 0.00
    y       1.73 +/- 0.00   1.7 +/- 0.00
    z       3.06 +/- 0.05   3.1 +/- 0.05
    xRot    -3.05 +/- 0.01  -3.1 +/- 0.01    
    yRot    -0.37 +/- 0.00  -0.38 +/- 0.00
    zRot    0.44 +/- 0.00   0.44 +/- 0.00
    GROUP8  Command Time    eStop
    x       -5.92 +/- 0.00  -5.9 +/- 0.00
    y       1.30 +/- 0.00   1.3 +/- 0.00
    z       4.27 +/- 0.04   4.2 +/- 0.04
    xRot    -2.99 +/- 0.01  -3.0 +/- 0.00
    yRot    -3.08 +/- 0.01  -3.1 +/- 0.01
    zRot    0.36 +/- 0.00   0.36 +/- 0.00
    ======= ==============  ================    
    
    
    
Tangent actuators
==================
