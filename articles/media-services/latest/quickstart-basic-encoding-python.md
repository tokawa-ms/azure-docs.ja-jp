---
title: Media Services と Python の基本的なエンコードのクイックスタート
description: このクイックスタートでは、Python と Azure Media Services で基本的なエンコードを行う方法について説明します。
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.date: 2/26/2021
ms.author: inhenkel
ms.openlocfilehash: ed6bad4e81a1af654357912f6fe0cd03a3fcd62e
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2021
ms.locfileid: "102500837"
---
# <a name="media-services-basic-encoding-with-python"></a>Python を使用した Media Services の基本的なエンコード

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="introduction"></a>はじめに

このクイックスタートでは、Python と Azure Media Services で基本的なエンコードを行う方法について説明します。 この例では、2020-05-01 Media Service v3 API を使用しています。

## <a name="prerequisites"></a>前提条件

- Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。
- このクイックスタートで使用するリソース グループを作成します。
- Media Services v3 アカウントを作成します。
- ストレージ アカウント キーを取得します。
- サービス プリンシパルとキーを作成します。

## <a name="get-the-sample"></a>サンプルを入手する

フォークを作成し、[Python サンプル リポジトリ](https://github.com/Azure-Samples/media-services-v3-python)にあるサンプルをクローンします。 このクイックスタートでは、BasicEncoding サンプルを使用します。

## <a name="create-the-env-file"></a>.env ファイルを作成する

アカウントから値を取得して *.env* ファイルを作成します。 つまり、拡張子のみで名前は付けずに保存します。  テンプレートとして *sample.env* を使用し、ローカル クローン内の BasicEncoder フォルダーに *.env* ファイルを保存します。

## <a name="try-the-code"></a>コードを試す

以下のコードは完全にコメント化されています。  スクリプト全体を使用するか、一部を抜粋して独自のスクリプトにご使用ください。

このサンプルでは、スクリプトの実行時に一緒に作成されたグループとして識別できるよう名前付けの目的で乱数が生成されます。  乱数は省略可能で、スクリプトのテストが完了したら削除してかまいません。

このサンプルでは、入力資産の SAS URL は使用していません。

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
from azure.mgmt.media import AzureMediaServices
from azure.mgmt.media.models import (
  Asset,
  Transform,
  TransformOutput,
  BuiltInStandardEncoderPreset,
  Job,
  JobInputAsset,
  JobOutputAsset)
import os, uuid, sys
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient, BlobClient

#Timer for checking job progress
import time

#This is only necessary for the random number generation
import random

# Set and get environment variables
# Open sample.env, edit the values there and save the file as .env
# (Not all of the values may be used in this sample code, but the .env file is reusable.)
# Use config to use the .env file.
print("Getting .env values")
client_id = os.getenv('AADCLIENTID','default_val')
key = os.getenv('AADSECRET','default_val')
tenant_id = os.getenv('AADTENANTID','default_val')
tenant_domain = os.getenv('AADTENANTDOMAIN','default_val') 
account_name = os.getenv('ACCOUNTNAME','default_val')
location = os.getenv('LOCATION','default_val')
resource_group_name = os.getenv('RESOURCEGROUP','default_val')
subscription_id = os.getenv('SUBSCRIPTIONID','default_val')
arm_audience = os.getenv('ARMAADAUDIENCE','default_val') 
arm_endpoint = os.getenv('ARMENDPOINT','default_val') 

#### STORAGE ####
# Values from .env and the blob url
# For this sample you will use the storage account key to create and access assets
# The SAS URL is not used here
storage_account_name = os.getenv('STORAGEACCOUNTNAME','default_val')
storage_account_key = os.getenv('STORAGEACCOUNTKEY','default_val')
storage_blob_url = 'https://' + storage_account_name + '.blob.core.windows.net/'

# Active Directory
LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

# Establish credentials
context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + tenant_id)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    RESOURCE,
    client_id,
    key
)

# The file you want to upload.  For this example, put the file in the same folder as this script. 
# The file ignite.mp4 has been provided for you. 
source_file = "ignite.mp4"

# Generate a random number that will be added to the naming of things so that you don't have to keep doing this during testing.
thisRandom = random.randint(0,9999)

# Set the attributes of the input Asset using the random number
in_asset_name = 'inputassetName' + str(thisRandom)
in_alternate_id = 'inputALTid' + str(thisRandom)
in_description = 'inputdescription' + str(thisRandom)
# Create an Asset object
# From the SDK
# Asset(*, alternate_id: str = None, description: str = None, container: str = None, storage_account_name: str = None, **kwargs) -> None
# The asset_id will be used for the container parameter for the storage SDK after the asset is created by the AMS client.
input_asset = Asset(alternate_id=in_alternate_id,description=in_description)

# Set the attributes of the output Asset using the random number
out_asset_name = 'outputassetName' + str(thisRandom)
out_alternate_id = 'outputALTid' + str(thisRandom)
out_description = 'outputdescription' + str(thisRandom)
# From the SDK
# Asset(*, alternate_id: str = None, description: str = None, container: str = None, storage_account_name: str = None, **kwargs) -> None
output_asset = Asset(alternate_id=out_alternate_id,description=out_description)

# The AMS Client
print("Creating AMS client")
# From SDK
# AzureMediaServices(credentials, subscription_id, base_url=None)
client = AzureMediaServices(credentials, subscription_id)

# Create an input Asset
print("Creating input asset " + in_asset_name)
# From SDK
# create_or_update(resource_group_name, account_name, asset_name, parameters, custom_headers=None, raw=False, **operation_config)
inputAsset = client.assets.create_or_update(resource_group_name, account_name, in_asset_name, input_asset)

# An AMS asset is a container with a specfic id that has "asset-" prepended to the GUID.
# So, you need to create the asset id to identify it as the container
# where Storage is to upload the video (as a block blob)
in_container = 'asset-' + inputAsset.asset_id

# create an output Asset
print("Creating output asset " + out_asset_name)
# From SDK
# create_or_update(resource_group_name, account_name, asset_name, parameters, custom_headers=None, raw=False, **operation_config)
outputAsset = client.assets.create_or_update(resource_group_name, account_name, out_asset_name, output_asset)

### Use the Storage SDK to upload the video ###
print("Uploading the file " + source_file)
# From SDK
# BlobServiceClient(account_url, credential=None, **kwargs)
blob_service_client = BlobServiceClient(account_url=storage_blob_url, credential=storage_account_key)
# From SDK
# get_blob_client(container, blob, snapshot=None)
blob_client = blob_service_client.get_blob_client(in_container,source_file)
# Upload the video to storage as a block blob
with open(source_file, "rb") as data:
  # From SDK
  # upload_blob(data, blob_type=<BlobType.BlockBlob: 'BlockBlob'>, length=None, metadata=None, **kwargs)
    blob_client.upload_blob(data, blob_type="BlockBlob")

### Create a Transform ###
transform_name='MyTrans' + str(thisRandom)
# From SDK
# TransformOutput(*, preset, on_error=None, relative_priority=None, **kwargs) -> None
transform_output = TransformOutput(preset=BuiltInStandardEncoderPreset(preset_name="AdaptiveStreaming"))
print("Creating transform " + transform_name)
# From SDK
# Create_or_update(resource_group_name, account_name, transform_name, outputs, description=None, custom_headers=None, raw=False, **operation_config)
transform = client.transforms.create_or_update(resource_group_name=resource_group_name,account_name=account_name,transform_name=transform_name,outputs=[transform_output])

### Create a Job ###
job_name = 'MyJob'+ str(thisRandom)
print("Creating job " + job_name)
files = (source_file)
# From SDK
# JobInputAsset(*, asset_name: str, label: str = None, files=None, **kwargs) -> None
input = JobInputAsset(asset_name=in_asset_name)
# From SDK
# JobOutputAsset(*, asset_name: str, **kwargs) -> None
outputs = JobOutputAsset(asset_name=out_asset_name)
# From SDK
# Job(*, input, outputs, description: str = None, priority=None, correlation_data=None, **kwargs) -> None
theJob = Job(input=input,outputs=[outputs])
# From SDK
# Create(resource_group_name, account_name, transform_name, job_name, parameters, custom_headers=None, raw=False, **operation_config)
job: Job = client.jobs.create(resource_group_name,account_name,transform_name,job_name,parameters=theJob)

### Check the progress of the job ### 
# From SDK
# get(resource_group_name, account_name, transform_name, job_name, custom_headers=None, raw=False, **operation_config)
job_state = client.jobs.get(resource_group_name,account_name,transform_name,job_name)
# First check
print("First job check")
print(job_state.state)

# Check the state of the job every 10 seconds. Adjust time_in_seconds = <how often you want to check for job state>
def countdown(t):
    while t: 
        mins, secs = divmod(t, 60) 
        timer = '{:02d}:{:02d}'.format(mins, secs) 
        print(timer, end="\r") 
        time.sleep(1) 
        t -= 1
    job_state = client.jobs.get(resource_group_name,account_name,transform_name,job_name)
    if(job_state.state != "Finished"):
      print(job_state.state)
      countdown(int(time_in_seconds))
    else:
      print(job_state.state)
time_in_seconds = 10
countdown(int(time_in_seconds))
```

## <a name="delete-resources"></a>リソースを削除する

クイックスタートを完了したら、リソース グループに作成したリソースを削除してください。

## <a name="next-steps"></a>次のステップ

[Media Services Python SDK](https://docs.microsoft.com/python/api/azure-mgmt-media/) について理解を深めます。