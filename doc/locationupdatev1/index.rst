.. include:: ../auth_url.rst

.. _locationupdate_behaviour:

LocationUpdate Operations API
-------------------------------------------------------------------------------

.. _locationupdate_behaviour_SetHomeLocation:

LocationUpdate SetHomeLocation
...............................................................................

Define the event parameters and store in /path/to/jsonfile:

.. code-block:: JSON

    {
      "operation": "SetHomeLocation",
      "behaviour": "LocationUpdate",
      "asset_attributes": {
        "arc_home_location_identity": "locations/90ec1b4f-93d9-49b4-b181-d6fe50ce2497"
      },
      "timestamp_declared": "2019-11-27T14:44:19Z",
      "principal_declared": {
        "issuer": "idp.synsation.io/1234",
        "subject": "phil.b",
        "email": "phil.b@synsation.io"
      }
    }

.. note::
    asset_attributes.arc_home_location_identity
       *Required* new home location for the asset

    timestamp_declared
        *Optional* Client-claimed time at which the maintenance was performed

    principal_declared
        *Optional* Client-claimed identity of person performing the operation

Add the LocationUpdate request to the Asset Record by POSTing it to the resource:

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
      "operation": "SetHomeLocation",
      "behaviour": "LocationUpdate",
      "asset_attributes": {
        "arc_home_location_identity": "locations/90ec1b4f-93d9-49b4-b181-d6fe50ce2497"
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


