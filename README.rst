TC Infrastructure Python Bindings
=================================

The tc-bbn-py project implements APIs for interfacing with the TC
infrastructure services.  It includes interfaces for data producers and data
consumers.  The goal is to have the API closely resemble the versions of the
API that are implemented in other languages; however, when there is a strong
conflict between language conventions, we may side with the language convention
over common API convention.  Python2 and Python3 are both supported.

Currently, the primary interface implemented in this project is a Kafka
producer and consumer, which is available under the tc.services package.  This
should handle serialization and validation internally for any callers.  If a
developer wants to experiment with the serialization framework, that is
available under the tc.schema.serialization package.

Installation
============
In order to install, you will need Python development headers for your target
version.  You will also need to install librdkafka.  We currently recommend
installing the latest tagged version from their github repo.  Once you have
those dependencies installed on your system, you can proceed.

Setuptools is used to manage installation of this project.  To install, run::

    pip install --process-dependency-links .

This will also take care of installing all other dependencies.

Unit Testing
============

To run the unit tests, obtain the source for the tc-bbn-py project.  From
there, one can navigate to the root directory of the project and run::

    python setup.py test

This should handle installing dependencies needed for the unit tests to run.
Unit tests are currently stored under the tc.tests package, although they are
not distributed with the project.  The choice was made to use a single
directory for all unit tests, but it is placed under the top level package for
namespacing purposes.  In particular, one of the dependencies currently
includes a top-level "tests" package, so this namespacing is required for now.

Examples
========

An example program showing how to use the existing Kafka Consumer and Producer
is available at examples/kafka_client_demo.py.  This example can generate
serialized dummy messages based on provided parameters and push them to a
kafka topic, and it can also consume from a topic, deserialize the messages,
and log them.  An example logging config is provided in examples/logging.conf.

Documentation
=============
In order to generate documentation, you need to install sphinx, and **you
should do so using pip**.  Sphinx very recently made changes to their default
template, and our documentation assumes that you have these changes in place.

Documentation can be generated by navigating to the doc directory and running::

    make html

Choice of Kafka Client
======================

We have moved to using confluent-kafka-python as the Kafka client of choice.
There are numerous reasons for this change:
 * It is supported by Confluent, who are basically the primary contributers
   to the Kafka project.  The expectation is that it will be supported in the
   long term.  This could possibly affect other Python Kafka client projects
   if they deem maintaining a separate project as being too much work.
 * It should have very good performance.  It directly wraps the librdkafka
   library, so under the hood most of the logic is implemented in C.
 * It is more likely to track the latest features.  Because this library
   wraps librdkafka, it should get the features provided by that library
   relatively quickly.  librdkafka in particular usually supports the latest
   features very quickly, as the primary developer is employed by Confluent.
   They keep features in librdkafka almost in sync with Kafka server releases.
   In contrast, other projects have been very slow to pick up the latest Kafka
   features.
