================================================
Environmental Modification in AgMIP Data Formats
================================================
:Version: 1.0.0-DRAFT
:Date: 2015-05-13

.. contents::

--------
Overview
--------

Some models *may* support environmental modifications which
allow variables to be mutated over time with model control. This
is beneficial for doing long-term runs or rotations. To provide
the models with this information from the AgMIP tools, the **Adjustments**
block is to be added to the root level of the ACE JSON Structure.

This is a DRAFT, please feel free to fork this on github and make
changes. Please submit a pull request for comment and merging into
the official document.

--------------
ACE Definition
--------------

The **adjustment** block has the ability to inform the translator
of the variable to be adjusted, how it is to be adjusted, by how much, at what schedule,
and, for layered data (soils, weather), at what level this adjustment is to take place.

A simple example is TMAX increased by two degrees celsius starting on the first weather
record:

.. code-block:: javascript

    adjustments: [{"variable":"tmax",
                   "method":"delta",
                   "date":"0000-01-01",
                   "value":"2",
                  }]

Each adjustment object in the list **must** contain:

variable
    ICASA variable name

method
    Method to apply: delta, multiply, or sub
    (where **delta** is to increase/decrease original value of the variable by the value given, **multiply** multiplies the orignal variable by the value and **sub** replaces the given variable by the given value.)


value
    The value to apply to the variable by the method

Each adjustment **can** contain:

date
    A fixed or relative date to apply on *(needs to the expanded upon)*

apply_to
    When dealing with layered data, which layer (by index) this modification is to apply to.

-------------
DOME Function
-------------

A modeler uses a DOME function in their DOME files to specify how to generate these
modifications.

This example shows how the function would be called to adjust the SLOC variable in the
upper 5cm of a soil profile.

.. code-block::

    NEW, SLOC, ADJUST(), MULT, 0.95, <5

How the date and layer information should be handled is still being discussed.
*This format needs to be expanded upon*
