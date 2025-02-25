.. _client-examples-kcat:

kcat: Command Example for |ak-tm|
=================================

In this tutorial, you will run a ``kcat`` (formerly known as ``kafkacat``) client application that produces
messages to and consumes messages from an |ak-tm| cluster.

.. include:: includes/client-example-overview.rst


Prerequisites
-------------

Client
~~~~~~

-  `kcat <https://github.com/edenhill/kcat>`__ installed on your
   machine. You must `build <https://github.com/edenhill/kcat#build>`__
   |kcat| from the latest master branch to get the ``-F`` functionality
   that makes it easy to pass in the configuration to your |ccloud|
   configuration file.

-  Local install of `Confluent Cloud CLI <https://docs.confluent.io/ccloud-cli/current/install.html>`__ v1.36.0 or later.


Kafka Cluster
~~~~~~~~~~~~~

.. include:: includes/client-example-prerequisites.rst


Setup
-----

#. .. include:: includes/clients-checkout.rst

#. Change directory to the example for |kcat|.

   .. code-block:: bash

      cd clients/cloud/kcat/

#. .. include:: includes/client-example-create-file-librdkafka.rst


Basic Producer and Consumer
---------------------------

.. include:: includes/producer-consumer-description.rst


Produce Records
~~~~~~~~~~~~~~~

#. Create the |ak| topic. If you are using |ccloud| then use the |ccloud| CLI:

   .. code-block:: text

      ccloud kafka topic create --if-not-exists test1

   If you are using a local |ak| cluster then use the |ak| CLI:

   .. code-block:: text

      kafka-topics --bootstrap-server lcoalhost:9092 --topic $topic_name --create --if-not-exists

#. Run |kcat|, writing messages to topic ``test1``, passing in arguments for:

   -  ``-F $HOME/.confluent/librdkafka.config``: configuration file for
      connecting to the |ccloud| cluster
   -  ``-K ,``: pass key and value, separated by a comma

   .. code-block:: bash

      kcat -F $HOME/.confluent/librdkafka.config -K , -P -t test1

#. Type a few messages, using a ``,`` as the separator between the message key
   and value:

   .. code-block:: text

      alice,{"count":0}
      alice,{"count":1}
      alice,{"count":2}

#. When you are done, press ``CTRL-D``.

#. View the :devx-examples:`producer code|clients/cloud/kcat/kcat-example.sh`.


Consume Records
~~~~~~~~~~~~~~~

#. Run |kcat| again, reading messages from topic ``test``, passing
   in arguments for:

   -  ``-F $HOME/.confluent/librdkafka.config``: configuration file for
      connecting to the |ccloud| cluster
   -  ``-K ,``: pass key and value, separated by a comma
   -  ``-e``: exit successfully when last message received

   .. code-block:: bash

      kcat -F $HOME/.confluent/librdkafka.config -K , -C -t test1 -e

   You should see the messages you typed earlier.

   .. code-block:: bash

         % Reading configuration from file $HOME/.confluent/librdkafka.config
         % Reached end of topic test1 [3] at offset 0
         alice,{"count":0}
         alice,{"count":1}
         alice,{"count":2}
         % Reached end of topic test1 [7] at offset 0
         % Reached end of topic test1 [4] at offset 0
         % Reached end of topic test1 [6] at offset 0
         % Reached end of topic test1 [5] at offset 0
         % Reached end of topic test1 [1] at offset 0
         % Reached end of topic test1 [2] at offset 0
         % Reached end of topic test1 [9] at offset 0
         % Reached end of topic test1 [10] at offset 0
         % Reached end of topic test1 [0] at offset 0
         % Reached end of topic test1 [8] at offset 0
         % Reached end of topic test1 [11] at offset 3: exiting

#. View the :devx-examples:`consumer code|clients/cloud/kcat/kcat-example.sh`.
