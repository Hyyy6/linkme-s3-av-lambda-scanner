# linkme-s3-av-lambda-scanner
Based on truework ClamAV S3 lambda scanner:
https://github.com/truework/lambda-s3-antivirus/

1. Setup clamav layer using Docker and run the following command, which would create layer.zip file in layer-src directory

       docker run --rm -v "$PWD"/layer-src:/tmp lambci/yumda:2 sh -c \
        'yum install -y clamav && cd /lambda/opt && rm -f /tmp/* && zip -yr /tmp/layer.zip .'

2. zip src folder and upload it as lambda functions source code

       zip -r ./lambda.zip linkme-virus-scanner/src/ 

3. Set up handlers, triggers and environment variables:

  For definitions update function:
   Set the handler to:
    
      download-definitions.lambdaHandleEvent
      
   Set the trigger for CloudWatch scheduled event
   
  For malware scanner function:
    Set the handler to:
    
      antivirus.lambdaHandleEvent

   Set the trigger to receive ObjectCreated events from s3.linkme.io bucket

  Set up environment variables:

     CLAMAV_BUCKET_NAME = clamav-definitions-bucket
     HOST_SERVER	= www.linkofile.com
     PATH_TO_AV_DEFINITIONS = lambda/s3-antivirus/av-definitions


4. set up layer with ClamAV binaries for both functions
