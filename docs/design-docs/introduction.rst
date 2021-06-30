###################
Design Introduction
###################

This set of documentation describes the philosopy that drove the design of Squonk 2
and the key concepts that are helpful to understand when using Squonk 2.

Infrastructure
==============

All the Squonk 2 components are designed to be deployed to a Kubernetes (K8S) platform which can be scaled to
whatever is needed. The expectation is that it will be relatively simple to deploy to individual customer
environments. 

Components
==========

Data Manager API
----------------

The Data Manager is the key component of Squonk 2 and provides a REST API for managing *datasets*,
including permissions, versioning and comprehensive metadata. Behind the REST API is an event driven
architecture involving RabbitMQ and Celery.

The API is compliant with the OpenAPI (aka Swagger) v3 specification and can be examined with the Swagger UI. 

Data Manager UI
---------------

This is the web based interface that will be used by end users. It allows to manage data and to execute jobs
and workflows. It is a React and NextJS based application.

Pose Viewer
-----------

An application that can be used to view and analyse virtual screening data, including the docked poses and
the interactions they make with the receptor.

Fragnet Search API and UI
-------------------------

Searching the fragment network data. This is currently a stand-alone application, but will be integrated into
the Squonk 2.9 ecosystem. You can find it `here <https://squonk.it/fragnet-search-ui>`_.

Concepts
========

The following concepts will help you understand how Squonk 2, and in particular the *Data Manager*, work.

Datasets
--------

These are files that the Data Manager manages. The Data Manager is strongly opinionated about these files in
terms of things like file extensions. Some types of files, such as those containing molecules, are indexed
so that substructure and similarity searches can be performed, and structures common between datasets can be
identified. For these indexed files each record is given a unique identifier, allowing records to be traced 
between datasets.

Datasets can be versioned and ownership and permissions are strongly controlled.

Datasets have strongly managed metadata, including *Annotations*, which provide the ability to fully describe
the dataset and to track its history.

Projects
--------

A project is a place where a user perfoms work, and acts as a collaboration space for multiple users.
Access to a project can be controlled by its creator or anyone who is made an editor.

It's key asset is a filesystem where the project's files are located. Datasets can be added to a project
for working with, and project's files can be added as datasets. That file system is accessible from any
applications or jobs that are launched. For instance you can launch a Jupyter Notebook and your projects
contents are immediately accessible.

Applications
------------

Applications can be launched in the context of a project, with the application having access to the project's
files. Currently the only type of application that is available is a Jupyter Notebook.

An application runs as the user's user ID and the project's group ID ensuring that access control is properly
managed.

Jobs
----

A job is a well defined service execution that processes data in a project. Like an application, a job has
access to the project's filesystem and and be used to process those files. Jobs can be simple or complex, small
or big, but executing them is very simple. Jobs needing lot's of compute power (e.g. docking workflows) are
parallelised and can scale to very large sizes if run on a suitable K8S cluster. Like applications, jobs run as
the users user ID  and the project's group ID ensuring that access control is properly managed.

Currently jobs are focussed around virtual screening workflows, but this will be expanded over time.
It is expected that the majority of services from the Squonk Computational Notebook will be ported over to
Squonk 2 jobs.

Workflows
---------

Workflows are not yet implemented, but will be a key component of Squonk 2. The will allow jobs to be combined
into a bigger workflow in a manner similar to the Squonk Computational Notebook.

Tasks
-----

When an application or job is created a task is created to track that execution. That task allows you to manage
the application or job, and see the events e.g. track the progress, look for any errors, view the reports or
logs.
