oc -n <hub-ns> exec deploy/<backstage> -c backstage-backend -- \
  curl -sS -x "$HTTPS_PROXY" -X POST \
  "https://login.microsoftonline.com/${TENANT}/oauth2/v2.0/token" \
  -d "client_id=..." -d "client_secret=..." \
  -d 'grant_type=client_credentials' \
  -d 'scope=https://graph.microsoft.com/.default'


  TENANT='<directory_id>'
CLIENT_ID='<application_id>'
CLIENT_SECRET='<client_value>'

curl -sS -X POST "https://login.microsoftonline.com/${TENANT}/oauth2/v2.0/token" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "client_id=${CLIENT_ID}" \
  -d "client_secret=${CLIENT_SECRET}" \
  -d 'grant_type=client_credentials' \
  -d 'scope=https://graph.microsoft.com/.default'
