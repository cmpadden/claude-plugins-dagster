# Monitoring & Alerting

Notification systems and monitoring platforms for pipeline observability, alerting, and incident management.

---

### Slack
**Package:** `dagster-slack` | **Support:** Dagster-supported

Send notifications and alerts to Slack channels for pipeline monitoring and team communication.

**Use cases:**
- Alert team on pipeline failures
- Send success notifications
- Share data quality reports
- Post daily pipeline summaries

**Quick start:**
```python
from dagster_slack import SlackResource

slack = SlackResource(
    token=dg.EnvVar("SLACK_BOT_TOKEN")
)

@dg.asset
def notify_completion(
    context: dg.AssetExecutionContext,
    slack: SlackResource
):
    slack.get_client().chat_postMessage(
        channel="#data-alerts",
        text=f"Asset {context.asset_key} completed successfully!"
    )
```

**Using sensors:**
```python
from dagster_slack import make_slack_on_run_failure_sensor

slack_failure_sensor = make_slack_on_run_failure_sensor(
    channel="#alerts",
    slack_token=dg.EnvVar("SLACK_BOT_TOKEN")
)
```

**Docs:** https://docs.dagster.io/integrations/libraries/slack

---

### PagerDuty
**Package:** `dagster-pagerduty` | **Support:** Dagster-supported

Create incidents and send alerts to PagerDuty for on-call incident management.

**Use cases:**
- Create incidents for critical pipeline failures
- Escalate data quality issues
- On-call alerting for production issues
- Integrate with incident response workflows

**Quick start:**
```python
from dagster_pagerduty import PagerDutyResource

pagerduty = PagerDutyResource(
    routing_key=dg.EnvVar("PAGERDUTY_ROUTING_KEY")
)

@dg.asset
def critical_alert(pagerduty: PagerDutyResource):
    pagerduty.trigger_incident(
        severity="critical",
        summary="Data pipeline failure in production",
        source="dagster-pipeline",
        custom_details={
            "pipeline": "daily_etl",
            "error": "Connection timeout"
        }
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/pagerduty

---

### Datadog
**Package:** `dagster-datadog` | **Support:** Dagster-supported

Send metrics, events, and logs to Datadog for comprehensive monitoring and observability.

**Use cases:**
- Track pipeline performance metrics
- Monitor asset materialization times
- Send custom metrics to Datadog
- Create dashboards for data operations

**Quick start:**
```python
from dagster_datadog import DatadogResource

datadog = DatadogResource(
    api_key=dg.EnvVar("DATADOG_API_KEY"),
    app_key=dg.EnvVar("DATADOG_APP_KEY")
)

@dg.asset
def monitored_asset(
    context: dg.AssetExecutionContext,
    datadog: DatadogResource
):
    import time
    start = time.time()

    # Process data
    result = process_data()

    # Send timing metric
    duration = time.time() - start
    datadog.get_client().metric.send(
        metric="dagster.asset.duration",
        points=[(time.time(), duration)],
        tags=[f"asset:{context.asset_key.path[-1]}"]
    )

    return result
```

**Docs:** https://docs.dagster.io/integrations/libraries/datadog

---

### Microsoft Teams
**Package:** `dagster-msteams` | **Support:** Dagster-supported

Send notifications to Microsoft Teams channels for pipeline monitoring.

**Use cases:**
- Alert teams using Microsoft Teams
- Send pipeline status updates
- Share data reports in Teams channels
- Enterprise communication integration

**Quick start:**
```python
from dagster_msteams import MSTeamsResource

teams = MSTeamsResource(
    hook_url=dg.EnvVar("MSTEAMS_WEBHOOK_URL")
)

@dg.asset
def teams_notification(teams: MSTeamsResource):
    teams.get_client().post_message(
        title="Pipeline Complete",
        message="Daily ETL pipeline finished successfully",
        theme_color="00FF00"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/msteams

---

### Twilio
**Package:** `dagster-twilio` | **Support:** Dagster-supported

Send SMS messages and make phone calls for critical alerts.

**Use cases:**
- SMS alerts for critical failures
- Phone call escalation for urgent issues
- Send verification codes
- Multi-channel alerting

**Quick start:**
```python
from dagster_twilio import TwilioResource

twilio = TwilioResource(
    account_sid=dg.EnvVar("TWILIO_ACCOUNT_SID"),
    auth_token=dg.EnvVar("TWILIO_AUTH_TOKEN")
)

@dg.asset
def critical_sms_alert(twilio: TwilioResource):
    twilio.get_client().messages.create(
        from_="+1234567890",
        to="+0987654321",
        body="CRITICAL: Production pipeline failed"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/twilio

---

### Prometheus
**Package:** `dagster-prometheus` | **Support:** Dagster-supported

Export Dagster metrics to Prometheus for time-series monitoring.

**Use cases:**
- Expose Dagster metrics to Prometheus
- Create Grafana dashboards
- Monitor pipeline health over time
- Track asset materialization rates

**Quick start:**
```python
from dagster_prometheus import PrometheusResource

prometheus = PrometheusResource(
    gateway_url="http://localhost:9091"
)

@dg.asset
def track_metric(prometheus: PrometheusResource):
    from prometheus_client import Counter

    counter = Counter(
        "dagster_assets_materialized",
        "Number of assets materialized"
    )
    counter.inc()

    # Push to gateway
    prometheus.push_to_gateway(
        job="dagster_pipeline",
        registry=counter._registry
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/prometheus

---

### Papertrail
**Package:** `dagster-papertrail` | **Support:** Community-supported

Send logs to Papertrail for centralized log management.

**Use cases:**
- Centralize Dagster logs
- Search and filter pipeline logs
- Set up log-based alerts
- Long-term log retention

**Quick start:**
```python
from dagster_papertrail import PapertrailResource

papertrail = PapertrailResource(
    host="logs.papertrailapp.com",
    port=12345
)

@dg.asset
def logged_asset(
    context: dg.AssetExecutionContext,
    papertrail: PapertrailResource
):
    papertrail.log_message(
        f"Processing asset {context.asset_key}",
        level="INFO"
    )
    return process_data()
```

**Docs:** https://docs.dagster.io/integrations/libraries/papertrail

---

### Apprise
**Package:** `dagster-apprise` | **Support:** Community-supported

Universal notification library supporting 80+ services (Discord, Telegram, Email, etc.).

**Use cases:**
- Send notifications to multiple services
- Use services without dedicated integrations
- Centralized notification configuration
- Multi-channel alerting

**Quick start:**
```python
from dagster_apprise import AppriseResource

apprise = AppriseResource(
    urls=[
        "discord://webhook_id/webhook_token",
        "mailto://user:pass@gmail.com"
    ]
)

@dg.asset
def multi_channel_alert(apprise: AppriseResource):
    apprise.notify(
        title="Pipeline Alert",
        body="Daily pipeline completed successfully"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/apprise

---

### DingTalk
**Package:** `dagster-dingtalk` | **Support:** Community-supported

Send notifications to DingTalk (popular in China) for team communication.

**Use cases:**
- Notify teams using DingTalk
- Send pipeline alerts in Chinese organizations
- Integration with Asian enterprise tools

**Quick start:**
```python
from dagster_dingtalk import DingTalkResource

dingtalk = DingTalkResource(
    webhook_url=dg.EnvVar("DINGTALK_WEBHOOK"),
    secret=dg.EnvVar("DINGTALK_SECRET")
)

@dg.asset
def dingtalk_notification(dingtalk: DingTalkResource):
    dingtalk.send_message(
        "Pipeline completed successfully"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/dingtalk

---

## Monitoring Tool Selection

| Tool | Best For | Type | Urgency |
|------|----------|------|---------|
| **Slack** | Team communication | Chat | Low-Medium |
| **MS Teams** | Enterprise Microsoft | Chat | Low-Medium |
| **PagerDuty** | On-call incidents | Incident Mgmt | High |
| **Twilio** | SMS/Voice alerts | Communication | Critical |
| **Datadog** | Metrics & dashboards | Observability | All levels |
| **Prometheus** | Time-series metrics | Metrics | All levels |
| **Papertrail** | Log management | Logging | All levels |
| **Apprise** | Multi-platform | Universal | All levels |

## Common Patterns

### Run Failure Sensor
```python
from dagster_slack import make_slack_on_run_failure_sensor

@dg.run_failure_sensor
def slack_on_failure(context: dg.RunFailureContext, slack: SlackResource):
    slack.get_client().chat_postMessage(
        channel="#alerts",
        text=f"❌ Run {context.run_id} failed!\n"
             f"Error: {context.failure_event.message}"
    )
```

### Custom Metrics
```python
@dg.asset
def monitored_pipeline(
    context: dg.AssetExecutionContext,
    datadog: DatadogResource
):
    # Track custom business metrics
    datadog.get_client().metric.send(
        metric="business.records_processed",
        points=[(time.time(), record_count)],
        tags=["env:prod", "team:data"]
    )
```

### Multi-Channel Alerting
```python
@dg.asset
def critical_pipeline(
    slack: SlackResource,
    pagerduty: PagerDutyResource,
    twilio: TwilioResource
):
    try:
        result = process_critical_data()
        slack.post_message("#alerts", "✅ Success")
        return result
    except Exception as e:
        # Alert multiple channels
        slack.post_message("#alerts", f"❌ Failure: {e}")
        pagerduty.trigger_incident(
            severity="critical",
            summary=str(e)
        )
        twilio.send_sms(
            to="+1234567890",
            body=f"CRITICAL: {e}"
        )
        raise
```

## Tips

- **Alert fatigue**: Don't alert on every event - focus on actionable alerts
- **Severity levels**: Use appropriate channels for different severity levels
- **Context**: Include relevant context (run ID, asset, error message) in alerts
- **Testing**: Test alerting integrations before production deployment
- **Sensors**: Use sensors for automatic alerting on run/asset failures
- **Dashboards**: Combine metrics tools (Datadog/Prometheus) with visualizations
- **Escalation**: Use PagerDuty/Twilio for critical issues requiring immediate action
- **Batching**: Consider batching non-critical notifications to reduce noise
