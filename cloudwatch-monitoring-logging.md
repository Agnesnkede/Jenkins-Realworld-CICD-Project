### Setup Tomcat Server and Configure The CloudWatch Agent For (Metrics and Logs)
#### A) Install the CloudWatch Unified Agent Using SSM (Run Command)
- Navigate to the `SSM Service` in your `Working Region`
- Click on `Quick Setup`
- Click on the `Create` option on `Host Management`
![SSMCloudWatchInstall!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/adavadvavdvavsfdfsd.png)
- Configuration Options
    - Amazon CloudWatch
    - Install and configure the CloudWatch agent: `Enable`
    - Update the CloudWatch agent once every 30 days: `Enable`
![SSMCloudWatchInstall!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/adavadvavdvavsfdfsdsd.png)
- Targets (make sure to select them `Manual` (however we can use tags as well))
    - Choose between deploying to the current Region or a custom set of Regions: Select `Current Region`
    - Choose how you want to target instances: Select `Manual`
    - Instances: Select your `Stage-Env` and `Prod-Env` instances
![SSMCloudWatchInstall!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/acdac.png)
- Navigate to `Run Command`
    - Click on `Run Command`
    - Command Document: Select `AWS-ConfigureAWSPackage`
    - Action: Select `Install`
    - Name: `AmazonCloudWatchAgent`  *(Make sure to provide this specific name)*
    - Target selection: Select `Choose instances manually`
        - Select your `Stage-Env` and `Prod-Env` instances
    - Output Options
        - Write command output to an Amazon S3 bucket: `Uncheck/Disable`
    - Click on `RUN`
![SSMCloudWatchInstall!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/SDVSS.png)

#### A) Configure the CloudWatch Agent (Metric and Log Collector (Collectd))
- Login to both the `Stage` and `Prod` VMs
- Run the following commands on both Environment instances/vms
```bash
## Install the Collectd
sudo amazon-linux-extras install collectd

## Execute/Run the CloudWatch Config Wizard
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

## Validate Configuration
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s

## Start the CloudWatch Agent
sudo systemctl start amazon-cloudwatch-agent
```
- CloudWatch Agent Configuration Interactive Wizard
![CWA1!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw1.png)
![CWA2!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw2.png)
![CWA3!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw3.png)
![CWA4!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw4.png)
![CWA5!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw5.png)
![CWA6!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/cw6.png)



















#### Step 3: View Application Logs on Splunk
- Login to your `Splunk Server` at http://Splunk-Server-IP:8000
- Click on `Search and Reporting` -->> `Data Summary` -->> Select any of the displayed `Environments Host` to visualize `App Logs`
![SplunkSetup4!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/raw/zdocs/images/Screen%20Shot%202023-04-29%20at%2011.39.03%20PM.png)

- Application Log Indexed
![SplunkSetup3!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/raw/zdocs/images/Screen%20Shot%202023-04-29%20at%2010.55.36%20PM.png)