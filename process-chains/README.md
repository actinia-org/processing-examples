# Collection of process chain

## Examples processing with demouser

In [demouser-examples](https://github.com/actinia-org/processing-examples/blob/main/process-chains/demouser-examples) there are a few examples that you can test on [actinia.mundialis.de](actinia.mundialis.de) with the actinia `demouser`.

Here is a small example of how to perform actinia processing with `curl` and `jq` in the terminal:
```
export actinia="https://actinia.mundialis.de"
export AUTH='-u demouser:gu3st!pa55w0rd'

# start process
curl ${AUTH} -X POST -H "Content-Type: application/json" -H "accept: application/json" "${actinia}/api/v3/locations/nc_spm_08/processing_export" -d @demouser-examples/pc_slope_ascpect.json > resp.json && cat resp.json | jq

# request status of process
STATUS_URL=$(cat resp.json | jq .urls.status | cut -d '"' -f 2)
curl ${AUTH} ${STATUS_URL} | jq

# download results
CURRENT_DIR=${PWD}
mkdir example-actinia-results
cd example-actinia-results
curl ${AUTH} ${STATUS_URL} > resp.json
RESULT_URL1=$(cat resp.json | jq .urls.resources[0] | cut -d '"' -f 2)
RESULT_URL2=$(cat resp.json | jq .urls.resources[1] | cut -d '"' -f 2)
TIF1=$(basename "${RESULT_URL1}")
TIF2=$(basename "${RESULT_URL2}")
curl ${AUTH} ${RESULT_URL1} --output ${TIF1}
curl ${AUTH} ${RESULT_URL2} --output ${TIF2}

# cleanup
cd ${CURRENT_DIR}
rm -r example-actinia-results
```

## Content

* [demouser-examples](https://github.com/actinia-org/processing-examples/blob/main/process-chains/demouser-examples): examples which can be processed with the `demouser` on [actinia.mundialis.de](actinia.mundialis.de)
