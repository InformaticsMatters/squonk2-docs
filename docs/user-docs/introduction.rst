#################
User Introduction
#################

The user documentation is currently fairly limited.

User Interface Components
=========================

There is a header bar and 3 main tabs that you work with.

Header
------

Use this to:

* log in and out
* create a new project
* delete an existing project
* select an existing project
* edit the details of a project e.g. users who are editors
* switch between the 3 main tabs
* go to the home page

Data tab
--------

If you have no project selected you see all the datasets that you have access to.
These can be filtered using the *Search* box.
Any of these datasets can be attached to a project you are an editor of. They can be 
attached in read-onloy or read-write modes.

New datasets can be uploaded. We are strict about file extensions e.g. a SDF file must have a
``.sdf`` extension, though all files can be gzipped and have an additional ``.gz`` extension (e.g. a gzipped
SDF must have the ``.sdf.gz`` extension).

If you have selected a project you see the contents of that project's filesystem and can perform
some basic operations on them.

Files in a project can be selected, which adds them the the *short list* that is used for job executions.

Files that were created from datasets are referred to as *managed* files, those that were created by activity
in the project are referred to as *unmanaged* files, but they can be turned into *managed* files which
results in them becoming a new dataset. If a *managed* file has been updated by activity in the project it
can be updated as a new *version* of that dataset.

Executions tab
--------------

This is where you launch applications or jobs. The available ones can be filtered using the Search
box. This filtering uses the name and the labels that are applied to the application or job.

Jobs typically need inputs, and these are selected from the *short list*, the files you have selected in
the Data tab.

When a job is run you are prompted for the required inputs and options and when specified you can launch the 
job. A task is created which appears in the Tasks tab.

Tasks tab
---------

The tasks tab lists all your current task, get information about the execution and to perform operations on the task,
such as terminating or deleting it. 

