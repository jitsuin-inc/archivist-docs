.. include:: ../auth_url.rst

.. note::
    A full description of the workflow regarding attaching a file to an asset
    or event is given in :ref:`intro_attachments`.

.. note::
    The following operations assume that an attachment has been uploaded to
    Archivist node using the API :ref:`blobsV1_upload`.
    This attachment uuid is generically referred to as
    ``blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`` in the following text.

    Each attachment has an associated hash value and the name of tha hash algorithm 
    used. 


.. _attachments_behaviour:

Attachments Operations API
-------------------------------------------------------------------------------

.. _attachments_behaviour_attach:

Attachments Attach
...............................................................................

Define the event parameters and store in /path/to/jsonfile:

.. code-block:: JSON

    {
      "operation": "Attach",
      "behaviour": "Attachments",
      "event_attributes": {
        "arc_append_attachments": [
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 1",
                "arc_hash_value": "jnwpjocoqsssnundwlqalsqiiqsqp;lpiwpldkndwwlskqaalijopjkokkkojijl",
                "arc_hash_alg": "sha256",
          },
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 2",
                "arc_hash_value": "042aea10a0f14f2d391373599be69d53a75dde9951fc3d3cd10b6100aa7a9f24",
                "arc_hash_alg": "sha256",
          }
        ]
      },
      "timestamp_declared": "2019-11-27T14:44:19Z",
      "principal_declared": {
        "issuer": "idp.synsation.io/1234",
        "subject": "phil.b",
        "email": "phil.b@synsation.io"
      }
    }

.. note::
    event_attributes.arc_append_attachments
        *Required* List with details of all attachments to be attached to an asset,
        each attachment details must have following four fields defined:

        arc_attachment_identity
            *Required* identity of an attachment
        arc_display_name
            *Required* display name of an attachment
        arc_hash_value
            *Required* hash of the attachment Content
        arc_hash_alg
            *Required* algorithm used to calculate the hash

    timestamp_declared
        *Optional* Client-claimed time at which the maintenance was performed

    principal_declared
        *Optional* Client-claimed identity of person performing the operation

Add the Attachments request to the Asset Record by POSTing it to the resource:

.. code-block:: shell

    $ curl -v -X POST \
        -H "@$BEARER_TOKEN_FILE" \
        -H "Content-type: application/json" \
        -d "@/path/to/jsonfile" \
        $URL/archivist/v2/assets/add30235-1424-4fda-840a-d5ef82c4c96f/events


The response is:

.. code-block:: JSON

    {
      "identity": "assets/add30235-1424-4fda-840a-d5ef82c4c96f/events/11bf5b37-e0b8-42e0-8dcf-dc8c4aefc000",
      "asset_identity": "assets/add30235-1424-4fda-840a-d5ef82c4c96f",
      "operation": "Attach",
      "behaviour": "Attachments",
      "event_attributes": {
        "arc_append_attachments": [
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 1",
                "arc_hash_value": "jnwpjocoqsssnundwlqalsqiiqsqp;lpiwpldkndwwlskqaalijopjkokkkojijl",
                "arc_hash_alg": "sha256",
          },
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 2",
                "arc_hash_value": "042aea10a0f14f2d391373599be69d53a75dde9951fc3d3cd10b6100aa7a9f24",
                "arc_hash_alg": "sha256",
          }
        ],
      },
      "asset_attributes": {
        "arc_attachments": [
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 1",
                "arc_hash_value": "jnwpjocoqsssnundwlqalsqiiqsqp;lpiwpldkndwwlskqaalijopjkokkkojijl",
                "arc_hash_alg": "sha256",
          },
          {
                "arc_attachment_identity": "blobs/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "arc_display_name": "an attachment 2",
                "arc_hash_value": "042aea10a0f14f2d391373599be69d53a75dde9951fc3d3cd10b6100aa7a9f24",
                "arc_hash_alg": "sha256",
          }
        ]
      },
      "timestamp_accepted": "2019-11-27T15:13:21Z",
      "timestamp_declared": "2019-11-27T14:44:19Z",
      "timestamp_committed": "2019-11-27T15:15:02Z",
      "principal_declared": {
        "issuer": "idp.synsation.io/1234",
        "subject": "phil.b",
        "email": "phil.b@synsation.io"
      },
      "principal_accepted": {
        "issuer": "job.idp.server/1234",
        "subject": "bob@job"
      },
      "confirmation_status": "CONFIRMED",
      "block_number": 12,
      "transaction_index": 5,
      "transaction_id": "0x07569"
    }

.. note::
    asset_attributes.arc_attachments
        Holds all asset attachments - pre-existing asset attachments
        and attachments provided in arc_append_attachments
