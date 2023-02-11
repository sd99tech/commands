# commands
finding S3 buckets 
#!/bin/bash

# List of possible S3 bucket permutations
buckets=(
  "$APP_NAME"
  "$COMPANY_NAME-$APP_NAME"
  "$APP_NAME-$COMPANY_NAME"
  "$COMPANY_NAME.$APP_NAME"
  "$APP_NAME.$COMPANY_NAME"
)

# Loop through the list of bucket permutations
for bucket in "${buckets[@]}"; do
  # Attempt to access the S3 bucket
  response=$(curl -sL -w "%{http_code}\\n" "http://$bucket.s3.amazonaws.com" -o /dev/null)

  # Check if the bucket is publicly accessible
  if [ "$response" == "200" ]; then
    echo "Publicly accessible S3 bucket found: $bucket"
  fi
done

