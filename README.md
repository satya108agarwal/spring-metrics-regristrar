# Java Example with Spring Security
Demonstrates how to expose endpoints with Spring Security so they can be reached by `metric-registrar`.

## Running the app locally
```bash
cd metric-registrar-examples/java-spring-security
./gradlew bootRun
```

### Push the app
```
cd metric-registrar-examples/java-spring-security
./gradlew build
cf push
```

## Endpoints
- `/simple` - Returns OK; exercises built-in Micrometer metrics
- `/high_latency` - a slow endpoint to simulate long-running requests
- `custom_metric` - increments the `custom_metric` counter
- `/actuator/prometheus` - Prometheus endpoint for metrics

## Running Tests
```bash
./gradlew test
```

# Register a Custom App Metrics endpoint

## Step 1: Install the Metric Registrar CLI:
```bash
cf install-plugin -r CF-Community "metric-registrar"
```
## Step 2: Register the metrics endpoint of the java-spring-security app by running:
```bash
cf register-metrics-endpoint spring-metrics-registrar /actuator/prometheus --insecure
```

## Step 3: Install the Log Cache CLI by running:
```bash 
cf install-plugin -r CF-Community "log-cache"
```
Log Cache is a component of TAS for VMs that caches logs and metrics from across the platform.


## Step 4 :View the app metrics as they are emitted running this command. The --follow flag appends output as metrics are emitted.
```bash 
cf tail spring-metrics-registrar --envelope-class metrics --follow
```
**Note:**
The output looks similar to the following example. The custom metric displays in the output.

Retrieving logs for app  in org sandbox / space development as example@user...
```bash 
2019-08-28T09:17:56.28-0700 [tutorial-example/1] GAUGE cpu:0.289158 percentage disk:135716864.000000 bytes disk_quota:1073741824.000000 bytes memory:399979315.000000 bytes memory_quota:2147483648.000000 bytes
2019-08-28T09:17:56.50-0700 [tutorial-example/0] GAUGE custom:1.000000
```

Refer: https://docs.vmware.com/en/VMware-Tanzu-Application-Service/6.0/tas-for-vms/autoscaler-spring-tutorial.html#sample-app-ui
