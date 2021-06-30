##############################
Managing Datasets with the API
##############################

Here we discuss Dataset management.

    The details of the API requests and responses can be found by inspecting
    the Data Manager's Swagger rendering of its OpenAPI definition and
    the details will not be repeated here.

****************
Adding a Dataset
****************

Datasets can be created from an external file or a file in an existing **Project**.
Once added, in order to use the Dataset in an *Operation* you need a
Project so that you can *attach* the Dataset to it.

Adding and attaching Datasets can be a time-consuming process, a process that's
accomplished internally using a **Task**.

When you add a Dataset it will be assigned an *Identity*, *Version* and a
*Task*. the Task will start automatically and it will manage the
upload of the file including the optional loading of molecular data
into the Squonk molecular database.

You will need to wait until the Dataset is *Ready* before you can use it.

Dataset API
===========

To add a Dataset from an external file::

    /dataset [POST]

To add a Dataset from a file in a Project::

    /dataset [PUT]

To inspect the state of a Dataset, i.e. check whether it's *Ready*::

    /dataset/{dataset_id}/{dataset_version}/info [GET]

Datasets can be added as new versions of an existing Dataset by naming the
original Dataset (and its version) during the POST or PUT.

Detailed information about the ongoing progress of a Dataset, which
will include its history of States and any diagnostic logging events that may
be created during the loading process, you need to use the ``/task`` endpoint,
providing the **Task** identity you were given when you added the Dataset::

    /task/{task_id} [GET]

Once a Dataset is *Ready* it can be added to a Project as a File::

    /file [POST]
