# Media Replay Engine Samples #

This repository contains sample Plugins (Lambda functions) and ML Model artifacts (Notebooks) that can be used in the process of building automated video clipping pipelines using the [Media Replay Engine](https://github.com/awslabs/aws-media-replay-engine) (MRE) framework. Besides sample Plugins and ML Models, a Fan Experience GUI is provided using which the highlights generated by MRE can be viewed from the fans' perspective.

There is also a sample HLS Harvester application included that can be used to stream HLS source content to an S3 bucket tied to the MRE processing pipeline.

This repository consists of the following applications, each of which can be deployed individually:

* `plugin-samples` - CDK to deploy all the sample Lambda functions into your account and register them as Plugins in MRE.
* `model-samples` - Contains the ML Model artifacts used by the sample Plugins. These need to be manually deployed and registered in MRE before deploying the sample Plugins.
* `fan-experience-frontend` - CDK to deploy the Fan Experience GUI into your account.
* `hls-harvester-sample` - CDK to deploy the HLS Harvester Sample application into your account.

# Install

## Prerequisites

* AWS MRE Framework (https://github.com/awslabs/aws-media-replay-engine)

## Build from scratch and deploy using AWS CDK

Run the following commands to build and deploy the sample applications from scratch. Be sure to define a value for `REGION` first.

```
REGION=[specify the region where MRE is deployed in a format like us-east-1]
git clone https://github.com/aws-samples/aws-media-replay-engine-samples.git
cd aws-media-replay-engine-samples
deployment_dir=$(pwd)/deployment
source_dir=$(pwd)/source
```

### plugin-samples

> **NOTE:** Please ensure that you have already deployed and registered all the sample ML Models present under `source/mre-model-samples/` in MRE before proceeding further. Failure to do so will impact the registration of one or more sample Plugins in MRE.

```
cd $deployment_dir
./build-and-deploy.sh --app plugin-samples --region $REGION [--profile <aws-profile>]
```

### fan-experience-frontend

```
cd $deployment_dir
./build-and-deploy.sh --app fan-experience-frontend --region $REGION [--profile <aws-profile>]
```

### hls-harvester-sample

```
cd $source_dir/mre-video-ingestion-samples/HLS_Harvester
cp harvester_config_template.py harvester_config.py
```

Open `harvester_config.py` and fill in the corresponding value for all the variables present. Once done, run the following commands to deploy the application:

```
cd $deployment_dir
./build-and-deploy.sh --app hls-harvester-sample --region $REGION [--profile <aws-profile>]
```

# Code Layout

| Path | Description |
|:---  |:------------|
| deployment/ |	shell scripts |
| deployment/build-and-deploy.sh | shell script to build and deploy plugin-samples and fan-experience-frontend applications using AWS CDK |
| source/ | source code folder |
| source/mre-plugin-samples/ | source code folder for the plugin-samples application |
| source/mre-plugin-samples/cdk/ | plugin-samples CDK application |
| source/mre-plugin-samples/LambdaLayers/ | Libraries to be manually imported as Lambda Layers |
| source/mre-plugin-samples/Plugins/ | Lambda code assets for all the sample Plugins |
| source/mre-model-samples/ | source code folder for the model-samples application |
| source/mre-model-samples/Models/ | Artifacts of all the sample ML Models |
| source/mre-fan-experience-frontend/ | source code for the fan-experience-frontend application |
| source/mre-video-ingestion-samples/HLS_Harvester/ | source code for the hls-harvester-sample application |

# Uninstall

## Option 1: Uninstall using AWS CDK
```
# Delete the plugin-samples application
cd MRE-Samples/source/mre-plugin-samples/cdk
cdk destroy [--profile <aws-profile>]

# Delete the fan-experience-frontend application
cd MRE-Samples/source/mre-fan-experience-frontend/cdk
cdk destroy [--profile <aws-profile>]

# Delete the hls-harvester-sample application
cd MRE-Samples/source/mre-video-ingestion-samples/HLS_Harvester/infrastructure
cdk destroy [--profile <aws-profile>]
```

## Option 2: Uninstall using the AWS Management Console
1. Sign in to the AWS CloudFormation console.
2. Select the MRE Plugin Samples stack.
3. Choose Delete.
4. Select the MRE Fan Experience Frontend stack.
5. Choose Delete.
6. Select the MRE HLS Harvester Sample stack.
7. Choose Delete.

## Option 3: Uninstall using AWS Command Line Interface
```
aws cloudformation delete-stack --stack-name <plugin-samples-stack-name> --region <aws-region>

aws cloudformation delete-stack --stack-name <fan-experience-frontend-stack-name> --region <aws-region>

aws cloudformation delete-stack --stack-name <hls-harvester-sample-stack-name> --region <aws-region>
```