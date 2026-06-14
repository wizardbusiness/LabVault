
```bash
curl https://www.wsdot.wa.gov/ferries/api/schedule/rest/routes/$(date +"%Y-%m-%d")?apiaccesscode={APIAccessCode} | jq '.[] | select(.Description == "Seattle \/ Bainbridge Island") | .RouteID' | xargs -I{} curl -s https://www.wsdot.wa.gov/Ferries/API/Schedule/rest/schedule/$(date +"%Y-%m-%d")/{}?apiaccesscode={APIACCESSCODE} | jq -r '["Ferry", "DepatureTime"], (.TerminalCombos.[].Times.[] | [.VesselName, (.DepartingTime | capture("/Date\\((?<ms>[0-9]+)").ms | tonumber / 1000 | strftime("%Y-%m-%d %I:%M %p"))]) | @tsv' | column -t -s$'\t'
```

