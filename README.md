# osqgen
Osquery helper tool for Elasticsearch 

#### Generate schema file from osquery repository

1. Go to osquery repo.
1. Checkout the tag for the osquery version that will be exported
1. Run `cmake .`
1. Run `python tools/codegen/genwebsitejson.py --specs=./specs > schema.json`

#### Generate fields for integration package

```
./osqgen --schema "./schema/osquery/osquery_schema_5.4.0.json" fields > osquery.yml
```
Write this file to `packages/osquery_manager/data_stream/result/fields/osquery.yml`

#### Generate fields for readme for kibana

This generates the markdown formatted osquery exported fields that can be added to `docs/reference/osquery-exported-fields.md` in kibana.

```
./osqgen --schema "./schema/osquery/osquery_schema_5.4.0.json" readme > readme.txt
```

#### Generate ECS fields for integration package

```
./osqgen --schema "./schema/ecs/fields.ecs_8.10.0.yml" ecs > ecs.yml
```

Currently this extracts out all the ```date```, ```ip```, ```long```, ```float```, ```boolean``` fields and writes them out in the integration package fields format.
The file ```schema/ecs/keep_fields.txt``` contains the list of fields that needs to be explicitly kept for ECS mapping file, without this the fields can be automapped incorrectly.
Add more fields there as needed.

For example:
```
- external: ecs
  name:  client.ip
- external: ecs
  name:  client.nat.ip
- external: ecs
  name:  code_signature.timestamp
- external: ecs
  name:  destination.ip
# ........
```

