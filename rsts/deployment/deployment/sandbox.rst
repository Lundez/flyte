.. _deployment-deployment-sandbox:

#########################
Sandbox Deployment
#########################

.. tags:: Kubernetes, Infrastructure, Basic

A sandbox deployment of Flyte is bundles togeether portable versions of Flyte's
dependencies such as a relational database and durable object store.

For the blob store requirements, Flyte Sandbox uses `Minio <https://min.io/>`__,
which offers an S3 compatible interface, and for Postgres, we use the stock
Postgres Docker image and Helm chart.

.. important::

    The sandbox deployment is not suitable for production environments. For instructions on how to create a
    production-ready Flyte deployment, checkout the :ref:`Deployment Paths <deployment-deployment>` guide.

*******************************************
Flyte Sandbox as a Single Docker Container
*******************************************

Flyte provides a way for creating a Flyte cluster as a self-contained Docker image. This is mini-replica of an
entire Flyte deployment, without the scalability and with minimal extensions.

The Flyte Sandbox can be run on any environment that supports containers and makes it extremely easy for users of Flyte
to try out the platform and get a feel for the user experience, all without having to understand Kubernetes or dabble
with configuration.

.. note::

   The Flyte single container sandbox is also used by the team to run continuous integration tests and used by the
   :ref:`cookbook:userguide`, :ref:`cookbook:tutorials` and :ref:`cookbook:integrations` documentation.

Requirements
============

- Install `kubectl <https://kubernetes.io/docs/tasks/tools/install-kubectl/>`__.
- Install `docker <https://docs.docker.com/engine/install/>`__ or any other OCI-compatible tool, like Podman or LXD.
- Install `flytectl <https://github.com/flyteorg/flytectl>`__, the official CLI for Flyte.

While Flyte can run any OCI-compatible task image, using the default Kubernetes container runtime (cri-o), the Flyte
core maintainers typically use Docker. Note that the ``flytectl demo`` command does rely on Docker APIs, but as this
demo environment is just one self-contained image, you can also run the image directly using another run time.

Within the single container environment, a mini Kubernetes cluster is installed using `k3s <https://k3s.io/>`__. K3s
uses an in-container Docker daemon, run using `docker-in-docker configuration <https://www.docker.com/blog/docker-can-now-run-within-docker/>`__
to orchestrate user containers.

Start the Sandbox
==================

To spin up a Flyte Sandbox, run:

.. prompt:: bash $

    flytectl demo start

This command runs a Docker container, which itself comes with a Docker registry
on ``localhost:30000`` so you can build images outside of the docker-in-docker
container by tagging your containers with ``localhost:30000/imgname:tag`` and
pushing the image.

The local Postgres installation is also available on port ``30001`` for users
who wish to dig deeper into the storage layer.

.. div:: shadow p-3 mb-8 rounded

   **Expected Output:**

   .. code-block::

      👨‍💻 Flyte is ready! Flyte UI is available at http://localhost:30080/console 🚀 🚀 🎉
      ❇️ Run the following command to export sandbox environment variables for accessing flytectl
      	export FLYTECTL_CONFIG=~/.flyte/config-sandbox.yaml
      🐋 Flyte sandbox ships with a Docker registry. Tag and push custom workflow images to localhost:30000
      📂 The Minio API is hosted on localhost:30002. Use http://localhost:30080/minio/login for Minio console

Now that you have the sandbox cluster running, you can now go to the :ref:`User Guide <cookbook:userguide>` or
:ref:`Tutorials <cookbook:tutorials>` to run tasks and workflows written in ``flytekit``, the Python SDK for Flyte.

**************************
Flyte Sandbox on the Cloud
**************************

Sometimes it's also helpful to be able to install a sandboxed environment on a cloud provider. That is, you have access
to an EKS or GKE cluster, but provisioning a separate database or blob storage bucket is harder because of a lack of
infrastructure support. Instructions for how to do this will be forthcoming.
