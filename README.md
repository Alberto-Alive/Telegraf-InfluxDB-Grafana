# Telegraf-InfluxDB-Graphana
Installation guide - Windows machine

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
 7. Configure OUTPUT: Change telegraf.conf file as it is just a template and we need to configure it so it can connect to your InfulxDB: go onto InfluxDB >> Data >> Telegraf >> InfluxDB Output Plugin and copy its contents into telegraf.conf file in your Telegraf folder.
 8. Now in this telegraf.conf file there is a line: token = "$INFLUX_TOKEN" where you need to change $INFLUX_TOKEN with a generated token using InfluxDB for a specific bucket you want: go onto InfluxDB >> Data >> API Tokens >> + Generate API Token (select your bucket then generate the token)
 9. Configure INPUT: Add to the telegraf.conf file the configs to let Telegraf know what data to monitor (can be found here): https://github.com/influxdata/telegraf/blob/release-1.21/plugins/inputs/cpu/README.md
 10. Make Telegraf run as a windows service by running this command in Powershell as Admin (run from: C:/Program Files/Telegraf): .\telegraf.exe --service install --config "C:\Program Files\Telegraf\telegraf.conf"
 11. Test Telegraf connecion by running this command in Powershell Admin (run from: C:/Program Files/Telegraf): .\telegraf.exe --config C:\"Program Files"\Telegraf\telegraf.conf --test
 12. Check if the service runs by running this command in Powershell Admin: net start
 13. Start telegraf to get data and send data by running this command in Powershell Admin: ./telegraf -config ./telegraf.conf

