<h1>Task - 1 : Run a simple Dataflow job</h1>


 bq mk lab

 gsutil cp gs://cloud-training/gsp323/lab.csv .

 cat lab.csv

 gsutil cp gs://cloud-training/gsp323/lab.schema .

 cat lab.schema



<h2>Task - 2 : Run a simple Dataproc job</h2>

This has to be done mannually.


<h2>Task - 3 : Run a simple Dataprep job</h2>

This has to be done mannually.

<h1>Task - 4 : AI</h1>


 gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"

 gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com

 export GOOGLE_APPLICATION_CREDENTIALS="/home/$USER/key.json"

 gcloud auth activate-service-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --key-file=$GOOGLE_APPLICATION_CREDENTIALS

 gcloud ml language analyze-entities --content="Old Norse texts portray Odin as one-eyed and long-bearded, frequently wielding a spear named Gungnir and wearing a cloak and a broad hat." > result.json

 gcloud auth login
 (Copy the token from the link provided)


 gsutil cp result.json gs://YOUR_PROJECT-marking/task4-cnl.result


<h3>Create an API key and export as API_KEY variable.</h3>

 export API_KEY={Replace with API KEY}

 nano request.json


<h3>Add this content:</h3>

<p>

{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-training/gsp323/task4.flac"
  }
}
  
</p>

-------

 curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
 "https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json

 gsutil cp result.json gs://YOUR_PROJECT-marking/task4-gcs.result


 gcloud iam service-accounts create quickstart

 gcloud iam service-accounts keys create key.json --iam-account quickstart@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com

 gcloud auth activate-service-account --key-file key.json

 export ACCESS_TOKEN=$(gcloud auth print-access-token)


 nano request.json


-------

Add this content:


{
   "inputUri":"gs://spls/gsp154/video/chicago.mp4",
   "features": [
       "TEXT_DETECTION"
   ]
}

<h3>Now add the following commands on the command line:</h3>


curl -s -H 'Content-Type: application/json' \
    -H "Authorization: Bearer $ACCESS_TOKEN" \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @request.json



curl -s -H 'Content-Type: application/json' -H "Authorization: Bearer $ACCESS_TOKEN" 'https://videointelligence.googleapis.com/v1/operations/OPERATION_FROM_PREVIOUS_REQUEST' > result1.json


gsutil cp result1.json gs://YOUR_PROJECT-marking/task4-gvi.result
