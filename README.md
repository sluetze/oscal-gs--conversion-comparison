# oscal-cli conversions

## Intro

Files without `-converted` are coming from the SdT-Bibliothek and are the source
Files get converted from json (primary format according to BSI) to the destination format
Files get converted from the destination format back to json
UUIDs of the different formats are compared to the source files
JSONs (original vs converted) are compared for differences

## YAML

`oscal-cli catalog convert --to=yaml 'Grundschutz++-Kompendium.json' 'Grundschutz++-Kompendium-converted.yaml'`
`oscal-cli catalog convert --to=json 'Grundschutz++-Kompendium-converted.yaml' 'Grundschutz++-Kompendium-converted-from-yaml.json'`

### YAML Comparison

```bash
# check UUID and compare it
cat Grundschutz++-Kompendium.json | jq .catalog.uuid                                                                                                                                                                                                                                                      ──(Fri,Dec05)─┘
"c398bda4-de8d-4d1e-8af8-b4902a921321"

cat Grundschutz++-Kompendium-converted.yaml | yq .catalog.uuid                                                                                                                                                                                                                                              ──(Fri,Dec05)─┘
c398bda4-de8d-4d1e-8af8-b4902a921321
# uuid is not touched and stays the same

# check content differences
diff Grundschutz++-Kompendium.json Grundschutz++-Kompendium-converted-from-yaml.json
# no differences!

# compare hash
sha256sum Grundschutz++-Kompendium*.json
8d47a9033f12388d82cf70649cfacca270bb4957375f397cb8b9d21695bad1b8  Grundschutz++-Kompendium-converted-from-yaml.json
8d47a9033f12388d82cf70649cfacca270bb4957375f397cb8b9d21695bad1b8  Grundschutz++-Kompendium.json
# Hash is the same, no conversion losses!
```

## XML

`oscal-cli catalog convert --to=xml 'Grundschutz++-Kompendium.json' 'Grundschutz++-Kompendium-converted.xml'`
`oscal-cli catalog convert --to=json 'Grundschutz++-Kompendium-converted.xml' 'Grundschutz++-Kompendium-converted-from-yaml.json'`

### XML Comparison

```bash
# check UUID and compare it
cat Grundschutz++-Kompendium.json | jq .catalog.uuid                                                                                                                                                                                                                                                      ──(Fri,Dec05)─┘
"c398bda4-de8d-4d1e-8af8-b4902a921321"

cat Grundschutz++-Kompendium-converted.xml | xq -x '/catalog/@uuid'                                                                                                                                                                                                                                     ──(Fri,Dec05)─┘
c398bda4-de8d-4d1e-8af8-b4902a921321
# uuid is not touched and stays the same

# check content differences
diff Grundschutz++-Kompendium.json Grundschutz++-Kompendium-converted-from-xml.json
diff Grundschutz++-Kompendium.json Grundschutz++-Kompendium-converted-from-xml.json | grep -v prose | less
# there are a couple of differences, which boil down to escape sequences for quotations and parantheses, quotations and whitespaces mostly in the prose elements
# my guess is, that using consistent upper quotations will solve this partly.

# did not do hash comparison because they must be different.
```
