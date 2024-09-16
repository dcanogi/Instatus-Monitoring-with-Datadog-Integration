# Instatus-Monitoring-with-Datadog-Integration
Instatus Monitoring with Datadog Integration
How to Set Up Instatus with Datadog Webhooks


## Step 1: Create an API Key in Datadog

Log in to your Datadog account. If you're new to Datadog, you’ll be prompted to install the Datadog agent first. 

Follow the instructions provided to install it on your server or container.

For Docker, use the following command after copying your API key:

```bash
docker run -d --name datadog-agent \
-e DD_API_KEY=your_api_key \
-e DD_SITE="datadoghq.com" \
-v /var/run/docker.sock:/var/run/docker.sock:ro \
-v /proc/:/host/proc/:ro \
-v /sys/fs/cgroup:/host/sys/fs/cgroup:ro \
gcr.io/datadoghq/agent:latest
```
Once the agent is installed and running, navigate to `Integrations -> APIs in Datadog`.

Click Create API Key, name it, and copy the key.

## Step 2: Set Up Webhooks in Datadog

In Datadog, navigate to `Integrations -> Webhooks`.

Click on Add Webhook and provide the details, such as the name of the webhook.

In URL, paste the URL you’ll get from Instatus (see the next step).

Save the webhook.

## Step 3: Get the Webhook URL from Instatus

Log in to your Instatus account.

Go to `Integrations -> Datadog`.

Copy the URL provided by Instatus for webhook integration.

## Step 4: Configure Datadog Monitor (Example: Monitoring Outline, Nginx, Redis, OIDC, Postgres, and Datadog)

In this step, we will create monitors using the following queries to track the status of your Docker containers for services like Outline, Nginx, Redis, OIDC, Postgres, and Datadog.

In Datadog, go to Monitors -> Create Monitor.

Use the following queries to monitor each service's Docker container and set up critical alerts for containers that have been unresponsive for more than 1 minute:

Datadog Agent:
`sum:docker.containers.running{docker_image:gcr.io/datadoghq/agent:7}`

Nginx:
`sum:docker.containers.running{docker_image:nginx}`

Outline:
`sum:docker.containers.running{docker_image:outlinewiki/outline:0.72.0-3}`

Postgres:
`sum:docker.containers.running{docker_image:postgres:15.2-alpine3.17}`

Redis:
`sum:docker.containers.running{docker_image:redis:latest}`

OIDC:
`sum:docker.containers.running{docker_image:vicalloy/oidc-server}`

Set up the following alert condition: Trigger an alert if the container has been unresponsive for more than 1 minute.

For example, you can use the following condition:

`sum:docker.containers.running{docker_image:<your_image>} by {container} < 1`

Then, set the threshold to trigger if the count is 0 for more than 1 minute.

Under Notifications, select Webhook and choose the one you created in Step 2.

Save each monitor after configuring it.

## Step 5: Test the Integration

Trigger a critical issue in one of your services (e.g., stop Nginx or Redis).

Verify that your Instatus page is automatically updated with the alert from Datadog.

You can also configure maintenance notices in Datadog for scheduled downtimes.

The final result:


