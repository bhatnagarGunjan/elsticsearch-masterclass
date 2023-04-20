# Elasticsearch and Kibana setup

## Elasticsearch

1. Download Elasticsearch from [here](https://www.elastic.co/downloads/elasticsearch) and extract the archive on your machine.
2. From the terminal, set the current directory to the extracted Elasticsearch directory.
3. Startup Elasticsearch with the following command:
  ```
  bin/elasticsearch
  ```

4. On your first startup, you will see the below generated tokens. Copy them!

- This token is needed for Kibana to communicate with Elasticsearch and is valid for 30 minutes.
- To regenerate the token, use the following command:

```
bin/elasticsearch-create-enrollment-token –scope kibana
```


## Kibana

1. Download Kibana from [here](https://www.elastic.co/downloads/kibana) and extract the archive.
2. Disable Gatekeeper for the Kibana directory. On macOS, Gatekeeper is a security mechanism that enforces code signing and verifies the downloads before allowing them to run. To disable Gatekeeper, go to the Kibana directory and run the following command:

```
xattr -d -r com.apple.quarantine {path/to/kibana}
```
 
 
3. Go to the extracted Kibana directory and start Kibana using the following command:

```
bin/kibana
```


4. Kibana will start on port 5601.
5. Open the URL produced on your browser and login with your superuser credentials.

## Troubleshooting

1. Resetting the elastic user password.
```
cd /path/to/elasticsearch
bin/elasticsearch-reset-password -u elastic
```
2. [Generate an enrollment token](https://www.elastic.co/guide/en/elasticsearch/reference/current/create-enrollment-token.html)
3. [Troubleshoot password setup](https://www.elastic.co/guide/en/elasticsearch/reference/current/trb-security-setup.html)
4. If you face the error "Port 5601 is already in use. Another instance of Kibana may be running!", kill the process running using the following command:
```
kill -15 <PID>
```

You can get the PID by running the command:

```
lsof -i tcp:<port>
```



