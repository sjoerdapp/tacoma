Epidemics on models
===================

In a temporal network model based on rates, it might happen that
in a simulation there's many events which then have to be passed 
to the Gillespie simulation function. This is rather heavy on memory.
Instead, one can simulate directly on the model without saving the
generated temporal network. So far, this is implemented for the
edge activity model only. It works as follows.

First, you create an instance of the model, e.g. 
:class:`_tacoma.ActivityModel` as

.. code:: python
    
    AM = tc.ActivityModel(N, # number of nodes
                          k/(N-1.), # network density
                          omega, # edge activity rate
                          save_temporal_network=False)

By default, ``save_temporal_network`` is ``False``, if you want
to save the generated temporal network, pass ``True`` instead.

Second, create an instance of an epidemic class, e.g.
:class:`_tacoma.SIS`.

.. code:: python

    SIS = tc.SIS(N, #number of nodes
                 tmax, # maximum time of the simulation
                 infection_rate,
                 recovery_rate,
                 number_of_initially_infected = int(N), # optional, default: 1
                 number_of_initially_vaccinated = 0, # optional, default: 0
                 seed = 792, # optional, default: randomly initiated
                )

Note that the seed passed to :class:`_tacoma.ActivityModel` is overwritten
with the random number generator (RNG) created in :class:`_tacoma.SIS`. 
This RNG is also used in the corresponding Gillespie simulation, which is
started and plotted as 

.. code:: python

    tc.gillespie_epidemics(AM, SIS)

    import matplotlib.pyplot as pl

    pl.step(SIS.time, SIS.I)


