# ServiceNow OnCall Notes

## Netcool Integration

- Integration - Netcool/Omnibus -> Setup -> Record -> Socket Monitors -> Port 4447 (Only on espwsnowint01)
- Netcool do it from UI, stop and start socket
- If MID Server not responding, login to the server and stop and start the service: snc_mid_espwsnowint01_prod
- Validate Port is open: Task Manager -> Resource Monitor -> Network
- If MID Server is down, validate if the service is running.
  1. Stop and Start the MiD Server Service
  2. Kill Java and Restart Java
  3. Reboot Server
  4. Validate after each step
- NOTE: ServiceNow instance may still show _Error_ even when it's working. Just make sure to check the port 4447 is up, and the MID Server Service is running.

## Mainspring

- Mainspring -> Contact Clemente or whomever is oncall for MONITORING
- Check ECC Queue if system is slow, delete error records with source = `http://stcroixweb.csmc.edu/IntegrationEngine/integrationEngine.svc/text`

## User Administration

- User Records Access Notes
- Duplicate Accout w Duplicate UserIDs
- Termed -> No access Allowed -> Company 20 -> Merge Accounts
- Feed delay causing data to be flipped
- User inactive too many days will be locked out of CS-Link
