##################################
Operating on Datasets with the API
##################################

Here we discuss Dataset *Operations*, or how to process and work with
Datasets using **Applications**, **Jobs** AND **Workflows**.

    The details of the API requests and responses can be found by inspecting
    the Data Manager's Swagger rendering of its OpenAPI definition and
    the details will not be repeated here.

*Operations* run in *Projects* and work with *Datasets*. An Operation might be
an **Application** (a Jupyter Notebook), a **Job** or **Workflow**
(sequenced execution of Jobs).

When you run an Operation it is referred to as an **Instance**.

    Whatever *Operation* you wish to accomplish you will first need a *Project*,
    and *Files* in the project that are derived from a previously uploaded
    *Dataset*.

What is an **Application**?
    An Application is managed by a Kubernetes Operator and is typically something that
    provides its own user-interface, exposed by a URL. Some applications are
    built into the Data Manager but you can also provide your own.

    When run, Applications normally run continuously and needs to be stopped
    (deleted) when you've finished with them.

What is a **Job**?
    A Job is an operation you run against a given Dataset. Jobs run to
    completion and normally don't need to be stopped. Jobs are managed by
    a Kubernetes Operator that's built into the Data Manager service.

What is a **Workflow**?
    A Workflow is a directed graph of connected Job Templates.

    A running Workflow is represented by an **Instance**. When each
    Job template in the workflow begins to run it is represented
    by its own Instance.

Applications API
================

To get a list of Available applications, in summary and detail::

    /application [GET]
    /application/{application_id} [GET]

To run an Application you first need to create a Template, which
defines Application-specific properties. Once you create a template
you can run the same application in multiple projects and allow others
to launch an application like yours::

    /application-template [POST]
    /application-template/{application_template_id} [DELETE]

To execute an application::

    /application-template/{application_template_id} [POST] <- used to be /instance [POST]

To inspect the state of an Application, i.e. check whether it's *Ready*
and obtain its URL you can use the **Instance** identity you were given
when you started the Application::

    /instance/{instance_id} [GET]

Information about all Application instances is available using the general
endpoint::

    /instance [GET]

This will return all Instances that you have access to and, unless filtered,
will also contain information about Jobs and Workflows (describe later)
that are running.

To stop an Application::

    /instance/{instance_id} [DELETE]

Detailed information about the ongoing progress of an Application, which
will include its history of States and any diagnostic logging events that are
produced, you need to use the ``/task`` endpoint, providing the **Task**
identity you were given when you started the Application::

    /task/{task_id} [GET]  <- no change

Jobs and the Job Template API
=============================

To run a Job, typically a short-lived container
that processes a Project File (a Dataset in a Project),
you need to create a **Job Template**.

Jobs are containerised processes that are registered with teh API
and act on Project files. To get a list of available Jobs, in summary and
detail::

    /job [GET]  <- no change
    /job/{job_id} [GET]  <- no change

You can also submit a new Job [#f1]_ or update existing Job definitions::

    /job [POST]
    /job/{job_id} [PATCH]

Before you can run a Job you need to create an execution template. The template
normally Once created you'll receive a *Job Template ID*. You create and delete
Job Templates using the ``/job-template`` endpoint::

    /job-template [POST]
    /job-template/{job_template_id} [DELETE]

To execute a Job template in a Project, where you're presented with an
**Instance** identity::

    /job-template/{job_template_id} [POST]

To inspect the state of a Job, i.e. check whether it's *Finished*, you use the
**Instance** identity you were given when you started the Job::

    /instance/{instance_id} [GET]

Jobs normally run-to-completion but you can also stop a Job is you need to::

    /instance/{instance_id} [DELETE]

Detailed information about the ongoing progress of a Job, which
will include its history of States and any diagnostic logging events it produces,
you need to use the ``/task`` endpoint, providing the **Task** identity
you were given when you started the Job::

    /task/{task_id} [GET]  <- no change

Workflow API
============

A workflow is a *connected series of Job Templates*. The workflow is defined
using a lists of Job template identities along with an execution *order*
declared using a *Directed Graph*. [#f2]_

To get the current summary and details of known Workflows::

    /workflow [GET]
    /workflow/{workflow_id} [GET]

To create, update (creating a new version) and delete Workflows::

    /workflow [POST]
    /workflow/{workflow_id} [PATCH]
    /workflow/{workflow_id} [DELETE]

To execute a Workflow, in a Project::

    /workflow/{workflow_id}/{workflow_version} [POST]

To inspect the state of a Workflow, i.e. check whether it's *Finished*
you need to ise the Instance ID you were given when you created it::

    /instance/{instance_id} [GET]

The state of the workflow will also contain the series of Jobs that
it will run, including the instance identity of each Job that is currently
running and the instance identities for Jobs that have also completed.

To inspect the state of a Workflow, i.e. check whether it's *Finished*,
you use the **Instance** identity you were given when you started it::

    /instance/{instance_id} [GET]

Workflows normally run-to-completion but you can also stop a Workflow
if you need to::

    /instance/{instance_id} [DELETE]


Detailed information about the ongoing progress of individual Jobs in the
Workflow, which will include its history of States and any diagnostic logging
events produced, you need to use the ``/task`` endpoint, providing the **Task**
identity for the Job you were given when you inspected the Workflow state::

    /task/{task_id} [GET]  <- no change

Workflow instances can be stopped (deleted) but instances of individual
Jobs managed by the workflow cannot. You can only stop Job instances if they
were started using a Job Template.

.. rubric:: Footnotes

.. [#f1]    Job Definitions are covered elsewhere in the Documentation

.. [#f2]    If we just want to run 'sequence' of Jobs, i.e. `a - b - c`
            we could simplify the whole process and use Celery's chaining activity,
            but we close control of Tasks, i.e. one task then manages multiple
            processes/containers. Although this is nice (clever) it is too restrictive
            (i.e. you can't define the chain as a graph).

