# How to install CloudWatch Agent on Amazon Linux and Configure for Memory Usage 

## Create IAM ROle

Navigate to IAM Role - Create a new Role - Attach this Policy "CloudWatchAgentServerPolicy" - name it as - EC2CloudWatchAgentServerRole. 

## Launch Amazon Linux2023 

Launch and SSH to Linux 2023 instance 

## Install CWA on Linux 2023

```
wget https://s3.us-east-2.amazonaws.com/amazoncloudwatch-agent-us-east-2/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm 
sudo rpm -U ./amazon-cloudwatch-agent.rpm
```

## Configure CloudWatch Agent wizard

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

### Parameters to be added

Will ask set of questions: 

   a.) On which OS are you planning to use the agent? - Linux 

   b.) Are you using EC2 or On-Premises hosts? - EC2 - 1 

   c.) Which user are you planning to run the agent? - root - 1 

   d.) Do you want to turn on StatsD daemon? - 1 

   e.) Which port do you want StatsD daemon to listen to? - default choice - 8125 

   f.) What is the collect interval for StatsD daemon? - 10s 

   g.) What is the aggregation interval for metrics collected by StatsD daemon? -60s 

   h.) Do you want to monitor metrics from CollectD? WARNING: CollectD must be installed or the Agent will fail to start - yes 

   i.) Do you want to monitor any host metrics? e.g. CPU, memory, etc. - yes 

   j.) Do you want to monitor cpu metrics per core? - yes 

   k.) Do you want to add ec2 dimensions (ImageId, InstanceId, InstanceType, AutoScalingGroupName) into all of your metrics if the info is available? - yes 

   l.) Do you want to aggregate ec2 dimensions (InstanceId)? - yes 

   m.) Would you like to collect your metrics at high resolution (sub-minute resolution)? This enables sub-minute resolution for all metrics, but you can customize for specific metrics in the output json file. - 60s 

   n.) Which default metrics config do you want? - Basic 

   o.)Are you satisfied with the above config? Note: it can be manually customized after the wizard completes to add additional items - yes 

   p.) Do you have any existing CloudWatch Log Agent - no 

   q.) Do you want to monitor any log files? - no 

   r.) Do you want to store the config in the SSM parameter store? - no 

## To prevent Error Parsing

```
sudo mkdir -p /usr/share/collectd/ 
sudo touch /usr/share/collectd/types.db
```

## To fetch the CloudWatch Agent logs

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s 
```

## To check the status

```
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

## To Setup CloudWatch metrics

Navigate to AWS CloudWatch console – Select All Metric 

![image](https://github.com/kohlidevops/aws-cwa-amazon-linux/assets/100069489/490acbf7-bebd-46de-8b23-5e04e229efb9)

## To create Alarm for Memory usage 

Create Alarm – Select Metrics – CWAgent – target instance 

![image](https://github.com/kohlidevops/aws-cwa-amazon-linux/assets/100069489/96c5e76f-0831-4c77-bb08-1d277016d836)

![image](https://github.com/kohlidevops/aws-cwa-amazon-linux/assets/100069489/48cf7fd6-c0d7-437e-a4f8-3272f01ec011)

![image](https://github.com/kohlidevops/aws-cwa-amazon-linux/assets/100069489/66d47879-72c4-449c-9053-78caa9879879)

Then Next to Create Alarm 
