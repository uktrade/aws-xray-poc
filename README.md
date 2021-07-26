# aws-xray-poc

This is a sample app to demostrate the basics of configuring aws x ray on GDS PaaS.

This functions by sending udp traces to a python x ray daemon which also runs in the space.

https://github.com/aws/aws-xray-daemon

The application needs to be bound with a network policies like below:

your-app        aws-x-ray-daemon      udp        2000    space          org

Once bound the app config required is set in your settings

In Installed Apps:  'aws_xray_sdk.ext.django'

VAR:  
XRAY_RECORDER = {
    'AWS_XRAY_DAEMON_ADDRESS': 'aws-x-ray-daemon.apps.internal:2000',
    'AUTO_INSTRUMENT': True,  # If turned on built-in database queries and template rendering will be recorded as subsegments
    'AWS_XRAY_CONTEXT_MISSING': 'LOG_ERROR',
    'PLUGINS': (),
    'SAMPLING': False,
    'SAMPLING_RULES': None,
    'AWS_XRAY_TRACING_NAME': name', # the segment name for segments generated from incoming requests
    'DYNAMIC_NAMING': None, # defines a pattern that host names should match
    'STREAMING_THRESHOLD': None, # defines when a segment starts to stream out its children subsegments
}


Finally to enable tracing in your view add the following decorator

from aws_xray_sdk.core import xray_recorder
@xray_recorder.capture('name')
