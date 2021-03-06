.. _tutoStreamlit:

HiPlot component for Streamlit
===============================


`Streamlit <https://www.streamlit.io/>`_ is an open-source app framework. It enables data scientists and machine learning engineers to create beautiful, performant apps in pure Python.


Getting started
----------------------------------

You'll need both Streamlit (>=0.63 for `components support <https://medium.com/streamlit/introducing-streamlit-components-d73f2092ae30>`_) and HiPlot (>=0.18)

>>> pip install -U streamlit hiplot


Displaying an :class:`hiplot.Experiment`
-----------------------------------------

Displaying an HiPlot experiment in Streamlit is very similar to how you would do it in a Jupyter notebook, except that you should call
:meth:`hiplot.Experiment.to_streamlit` before calling :meth:`hiplot.Experiment.display`.



.. note:: :meth:`hiplot.Experiment.to_streamlit` has a ``key`` parameter, that can
    be used to assign your component a fixed identity if you want to change its
    arguments over time and not have it be re-created.

    If you remove the ``key`` argument in the example below, then the component will
    be re-created whenever any slider changes, and lose its current configuration/state.



.. literalinclude:: ../examples/demo_streamlit.py
    :start-after: DEMO_STREAMLIT_BEGIN

.. figure:: ../assets/streamlit.png


.. _tutoStreamlitRetValues:

HiPlot component return values
-----------------------------------

HiPlot is highly interactive, and there are multiple values/information that can be returned, depending on what the user provides for the parameter ``ret`` in :meth:`hiplot.Experiment.to_streamlit`

- ``ret="filtered_uids"``: returns a list of uid for filtered datapoints. Filtered datapoints change when the user clicks on *Keep* or *Exclude* buttons.
- ``ret="selected_uids"``: returns a list of uid for selected datapoints. Selected datapoints correspond to currently visible points (for example when slicing in the parallel plot) - it's a subset of filtered datapoints.
- ``ret="brush_extents"``: returns information about current brush extents in the parallel plot
- or a list containing several values above. In that case, HiPlot will return a list with the return values


.. code-block:: python


    xp.to_streamlit(key="hip1").display()  # Does not return anything
    filtered_uids = xp.to_streamlit(ret="filtered_uids", key="hip2").display()
    filtered_uids, selected_uids = xp.to_streamlit(ret=["filtered_uids", "selected_uids"], key="hip3").display()



.. _tutoStreamlitCache:

Improving performance with streamlit caching (EXPERIMENTAL)
-----------------------------------------------------------

Generating / displaying a large HiPlot Experiment / component can take a significant amount of time and bandwidth. Several things can speed up streamlit iterations:

- A data compression mode, which represents the underlying data more efficiently if most of the rows have the same columns
- Using streamlit's caching to store a frozen copy of the experiment



.. literalinclude:: ../examples/demo_streamlit_cache.py
    :start-after: DEMO_STREAMLIT_BEGIN
