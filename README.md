# Telegraf-InfluxDB-Graphana
Installation guide - Windows machine

InfluxDB v2.1.1 - Windows Binaries (64-bit)
 1. Go to:https://portal.influxdata.com/downloads/
 2. Select the version and platform
 3. Displayed will see 2 things: - SHA256 hash function to verify your software is genuine 
                                 - box of two line commands: - wget ...
                                                             - Expand-Archive ...
 4. Open PowerShell as Admin
 5. Run: - wget ... (downloads)
         - Expand-Archive ... (unzips)
 6. Your InfluxDB should be located in: C:/Program Files/InfluxDB
 7. Run the following command using admin Powershell to start InfluxDB on localhost:8086 (path:C:/Program Files/InfluxDB): .\influxdb.exe

Telegraf v1.21.4 - Windows Binaries (64-bit)
 1. Go to:https://portal.influxdata.com/downloads/
 2. Select the version and platform
 3. Displayed will see 2 things: - SHA256 hash function to verify your software is genuine 
                                 - box of two line commands: - wget ...
                                                             - Expand-Archive ...
 4. Open PowerShell as Admin
 5. Run: - wget ... (downloads)
         - Expand-Archive ... (unzips)
 6. Now make sure the path to your Telegraf looks like this (just because running some commands will look by default to this path for the telegraf.conf file): C:/Program Files/Telegraf/- telegraf.conf
          - telegraf.exe
 7. Set up is available here : https://docs.influxdata.com/influxdb/v2.1/write-data/no-code/use-telegraf/manual-config/?t=Windows
 8. Configure OUTPUT: Change telegraf.conf file as it is just a template and we need to configure it so it can connect to your InfulxDB: go onto InfluxDB >> Data >> Telegraf >> InfluxDB Output Plugin and copy its contents into telegraf.conf file in your Telegraf folder.
 9. Now in this telegraf.conf file there is a line: token = "$INFLUX_TOKEN" where you need to change $INFLUX_TOKEN with a generated token using InfluxDB for a specific bucket you want: go onto InfluxDB >> Data >> API Tokens >> + Generate API Token (select your bucket then generate the token)
 10. Configure INPUT: Add to the telegraf.conf file the configs to let Telegraf know what data to monitor (can be found here): https://github.com/influxdata/telegraf/blob/release-1.21/plugins/inputs/cpu/README.md
 11. Make Telegraf run as a windows service by running this command in Powershell as Admin (run from: C:/Program Files/Telegraf): .\telegraf.exe --service install --config "C:\Program Files\Telegraf\telegraf.conf"
 12. Test Telegraf connecion by running this command in Powershell Admin (run from: C:/Program Files/Telegraf): .\telegraf.exe --config C:\"Program Files"\Telegraf\telegraf.conf --test
 13. Check if the service runs by running this command in Powershell Admin: net start
 14. Start telegraf to get data and send data by running this command in Powershell Admin: ./telegraf -config ./telegraf.conf

Graphana v8.4.4 - Windows grafana-enterprise-8.4.4.windows-amd64.msi
1. Download Graphana for Windows from: https://grafana.com/grafana/download?platform=windows
2. Extract Graphana in C:/Program Files/Graphana
3. Run Graphana by running this command in Powershell Admin (run from: C:/Program Files/Graphana/bin): .\grafana-cli.exe
4. Go to: localhost:3000
5. To set up InfluxDB on Graphana use the following instructions (we'll sum up in the next steps): https://docs.influxdata.com/influxdb/v2.1/tools/grafana/?t=InfluxQL
6. Firstly, create a datasource a.k. instruct Graphana where to take your data from: in Graphana >> Configuration >> Data sources >> Add data source >> Select InfluxDB >> Follow setp 7
7. Set up InfluxDB config on Graphana
        - HTTP.URL: http://localhost:8086/
        - HTTP.Access: Server(default)
        - Custom_HTTP_Headers.Header: Authorization
        - Custom_HTTP_Headers.Value: Token <Token-that-is-generated-taken-from-InfluxDB-API_Tokens>
        - InfluxDB_Details.Database: Database_name
        - InfluxDB_Details.User: User_name
        - InfluxDB_Details.Password: Password
        - InfluxDB_Details.HTTP_method: GET
8. Click Save & test (Alert: Data source is working; if Alert: Error: Bad request >> you would need to check the Custom_HTTP_Headers -> Value should look like: Token y0uR5uP3rSecr3tT0k3n)
