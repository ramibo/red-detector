![red-detector](static/red-detector.png)

# Red-Detector

## Description
Scan your EC2 instance to find its vulnerabilities using Vuls (https://vuls.io/en/).

Audit your EC2 instance to find security misconfigurations using Lynis (https://cisofy.com/solutions/#lynis).

Scan your EC2 instance for signs of a rootkit using Chkrootkit (http://www.chkrootkit.org/).
![](static/vuls-gif.gif)


## Requirements
1. Configured AWS account with the EC2 actions mentioned below. The policy containing these requirements can be found in red-detector-policy.json.

Actions details:

| Required action premission | Why it is required |
| --------------------- | ------------------------------------------ |
| "AttachVolume" | Enables attaching the volume with the taken snapshot to the EC2 instance that is being used for the vulnerabilities scan. |
| "AuthorizeSecurityGroupIngress" | Enables attaching security group to the EC2 instance. Contains IP premmisions to ssh port and a random port generated for the scan UI access. |
| "DescribeInstances" | Enables access to the clients EC2 instances details. |
| "CreateKeyPair" | Enables the creation of a key pair that is being used as the key of the EC2 instance. |
| "CreateTags" | Enabled the creation of Tags on the Volume and Snapshot. |
| "DescribeRegions" | Enables access to the clients active regions to enable the user select the relevant one for the scan. |
| "RunInstances" | Enables the creation of an EC2 instance under the users client. |
| "ReportInstanceStatus" | Enables getting the current status of the created EC2 instance to make sure it is running. |
| "DescribeSnapshots" | Enables getting the current status of the taken snapshot to make sure it is available. |
| "DescribeImages" | Enables querying AMI's to get the latest Ubuntu AMI. |
| "DescribeVolumeStatus" | Enables getting the current status of the volume being created. |
| "DescribeVolumes" | Enables getting details about a volume. |
| "CreateVolume" | Enables the creation of a volume, in order to attach it the taken snapshot and attach it to the EC2 instance used for the vulnerabilities scan. |
| "DescribeAvailabilityZones" | Enables access to the clients active availability zones to select one for the created volume that is being attach to the EC2 instance. |
| "DescribeVpcs" | Enables getting the clients default vpc. Used for the EC2s security group generation. |
| "CreateSecurityGroup" | Enables the creation of a security group that is being attached to the EC2 instance. |
| "CreateSnapshot" | Enables taking a snapshot. Used to take a snapshot of the chosen EC2 instance. |
| "DeleteSnapshot" | Enables deleting the stale snapshot was created during the process |
 

2. Running EC2 instance - Make sure you know the region and instance id of the EC2 instance you would like to scan.
Supported versions:
    - Ubuntu: 14, 16, 18, 19, 20
    - Debian: 6, 8, 9
    - Redhat: 7, 8
    - Suse: 12
    - Amazon: 2
    - Oracle: 8


## Installation
```bash
sudo git clone https://github.com/lightspin-tech/red-detector.git
pip3 install -r requirements.txt
```



## Usage
### Interactive
```bash
python3 main.py
```
### Command arguments
```bash
usage: main.py [-h] [--region REGION] [--instance-id INSTANCE_ID] [--keypair KEYPAIR] [--log-level LOG_LEVEL]

optional arguments:
  -h, --help                show this help message and exit
  --profile-name            profile name
  --region REGION           region name
  --instance-id INSTANCE_ID EC2 instance id
  --keypair KEYPAIR         existing key pair name
  --log-level LOG_LEVEL log level
```

## Flow
1. Run main.py.
2. Region selection: use default region (us-east-1) or select a region.
    Notice that if the selected region does not contain any EC2 instances you will be asked to choose another region.
2. EC2 inatance-id selection: you will get a list of all EC2 instances ids under your selected region and you will be asked to choose the inatance you would like to scan.
    Make sure to choose a valide answer (the number left to the desired id).
3. Track the process progress... It takes about 30 minutes.
4. Get a link to your report!

## Troubleshooting
### verbouse logging
```python3 main.py --log-level DEBUG```
### scanners databases update process
1. connect to the EC2 instance created ```ssh ubuntu@PUBLICIP -i KEYPAIR.pem```
2. watch the progress ```tail /var/log/user-data.log```

### Contact Us
For technical information, contact us at support@lightspin.io.

Want to see this capability on steroids? Check [Lightspin.io](https://lightspin.io)

## License
This repository is available under the [Apache License 2.0](https://github.com/lightspin-tech/red-detector/blob/main/LICENSE).
