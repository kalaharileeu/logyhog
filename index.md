## Overview

Logyhog is a IOT dashboard. Logyhog was made to help monitor 100's of iot devices at a time. With features like pass/fail, y data, xy data and text data.

### Usage

The main purpose of Logyhog is to show PASS or FAIL for 100’s of nodes or units under test.
The user can then look at the FAILED node/unit Y, XY or text data with the Logyhog dashboard to investigate further.

### Dashboard

![Example Dashboard](images/sets_one_fail.png)

A example dashboard showing eleven devices. The below code creates this simple dashboard.

```python

# Import the library that logs to the logyhog api
from httppythonlog.HttPython import IP_token_Task_User

task_token = "24-JJPFQ7Q"
task_name = "MyUnitsInTest"
host = "https://api.logyhog.com"
# My fake devices under test
myDevices = ["unit1", "unit2", "unit3", "unit4", "unit5", "unit6", "unit7", "unit8", "unit9", "unit10", "unit11"]
# Test good data. Some data (Volt, Current)
ThisDataPass = [(190, 11), (170, 12), (177, 13), (188, 11)]
# Test fail data. Some data (Volt, Current)
ThisDataFail = [(190, 11), (170, 12), (400, 13), (300, 11)]

if __name__ == "__main__":
    # Task token and name added, and the location where logyhog is running, could be localhost
    logyhogLogger = IP_token_Task_User(host, task_token, task_name)
    # Set XY data limits, for example voltage and current limits of the data
    for myDevice in myDevices:
        # dataset_name, x HighLimit, y HighLimit, x LowLimit, y Lowlimit
        logyhogLogger.setlimits_dataset(myDevice, 200, 15, 150, 10)

    # Ready to log xy data for limit checks
    for myDevice in myDevices:
        # Logging pass data to these devices
        for xyData in ThisDataPass:
            print(".")
            logyhogLogger.xy(myDevice, xyData[0], xyData[1])
    # Make this unit fail, by loading out of limit data
    for xyFailData in ThisDataFail:
        logyhogLogger.xy("unit7", xyFailData[0], xyFailData[1])
# Done
```

### Y plots

### XY plots

### Text Data

### Run Environment

Logyhog is deployed to an Kubenetes cluster or on Kubernetes on Windows that runs docker desktop. This project contains the deployment files and the instructions to do so.

## How to start Logyhog Kubernetes (Docker desktop)

### Create the ingress controller

```powershell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
```

### Setup the configmaps

```powershell
kubectl create configmap logmini --from-file=logmini_appsettings

kubectl create configmap logstuff --from-file=logstuff_appsettings
```

### Deploy the test op the application

```powershell
kubectl apply -f .
```

Note: the "." after the -f

# Steps to your first logged data

1. You will need to have a running instance of Logyhog.
2. You will need a copy of the Python 3 driver that will enable you to log data.
(As of 24 January 2022. My NetStandard 2.1 driver is out of date. I'm working to fix that.)
