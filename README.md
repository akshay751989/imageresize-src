This Python script is a Serverless Image Thumbnail Generator intended to run as an AWS Lambda function. Here's a breakdown of what it does and how it works:

🔧 Primary Purpose
It listens for S3 events (typically when a new image is uploaded to an S3 bucket), then:

Downloads the image from the source bucket.

Creates resized copies of the image (e.g., thumbnail, profile, cover sizes).

Uploads the resized images to a destination S3 bucket under size-specific folder paths.

🧱 Main Components
1. Global Configuration
python
Copy
Edit
set_global_vars()
Sets owner, environment, region, and various image sizes (e.g., thumbnail, profile, etc.).

2. Extension Validation
python
Copy
Edit
is_extension_valid(key)
Ensures only .jpg, .jpeg, or .png images are processed.

3. Image Processing Logic
python
Copy
Edit
img_resize_factory(src_path, tgt_path, size)
Resizes the image using Pillow (PIL.Image) and saves it.

4. S3 Operations
python
Copy
Edit
_get_img_from_s3(bucket, key, download_path)
_put_img_to_s3(upload_path, bucket, key)
Download from and upload to AWS S3.

5. Resize and Save All Versions
python
Copy
Edit
_resize_factory_assembly(key, src_bucket, des_bucket)
For each defined image size:

Downloads the image to /tmp/ directory (as required by AWS Lambda).

Resizes it.

Uploads the resized image to a specific folder in the destination bucket.

🚀 Lambda Handler
python
Copy
Edit
lambda_handler(event, context)
Triggered by S3 events.

Extracts bucket name and object key from the event.

Calls the image resize factory.

🌐 Execution Context
This script is designed to be deployed on AWS Lambda.

It relies on:

S3 event triggers.

Environment variable DESTINATION_BUCKET to specify the output bucket.

📌 Key Notes
All image processing happens in the Lambda /tmp directory, due to Lambda's storage limitations.

If source and destination buckets are the same, it may cause an infinite loop unless handled with S3 event filters.

It logs errors and returns JSON-style status responses.
