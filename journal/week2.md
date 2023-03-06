# Week 2 — Distributed Tracing

## Instrument Honeycomb with OTEL
I created  a free honeycomb account and included the api key in my project

```
export HONEYCOMB_API_KEY="my-honeycomb-api-key"
gp env HONEYCOMB_API_KEY="my-honeycomb-api-key"
```
```
gp env HONEYCOMB_SERVICE_NAME="backend-flask"
export HONEYCOMB_SERVICE_NAME="backend-flask"
```

#### Add the following environment variables to docker compose
```
OTEL_SERVICE_NAME: "backend-flask"
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```
#### paste the following in requirements.txt:
```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```

#### add to app.py:-
```
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```

#### Initialize tracing and an exporter that can send data to Honeycomb
```python
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

#### Initialize automatic instrumentation with Flask
```python
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```


      span = trace.get_current_span()
      now = datetime.now(timezone.utc).astimezone()
      span.set_attribute("app.now", now.isoformat())


![Proof of honeycomb instrumentation](assets/HONEYCOMB.png)

***
## Instrument AWS X-Ray
Log into aws account, and search for xray.

Back to the application code,
```
cd into frontend
npm install
```
change directory to backend add to the requirements.txt

```
aws-xray-sdk
```
Export environment variable:
```
export AWS_REGION="us-east-1"
gp env AWS_REGION="us-east-1"
```
then run cli command to create xray group
```
aws xray create-group \
   --group-name "Cruddur" \
   --filter-expression "service(\"backend-flask\") {fault OR error}"
```

A container is needed to run a deamon service for xray to communicate with 

Add Deamon Service to Docker Compose
```
  xray-daemon:
    image: "amazon/aws-xray-daemon"
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "us-east-1"
    command:
      - "xray -o -b xray-daemon:2000"
    ports:
      - 2000:2000/udp
```

We need to add these two env vars to our backend-flask in our docker-compose.yml file

      AWS_XRAY_URL: "*4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}*"
      AWS_XRAY_DAEMON_ADDRESS: "xray-daemon:2000"


![cruddur xray traces from my account](assets/xray-trace.png)
![cruddur xray traces from my account](assets/xray-trace2.png)






