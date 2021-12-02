# Indian Institute of Technology, Jodhpur
## Virtualization and Cloud Computing: CSL7510

## Endsem Project
### Team members:
**Dipinti Manandhar (M20CS020)**
**Umang Rajendra Barbhaya (M20CS017)**
### Course Instructor:
**Dr. Sumit Kalra**
**Assistant Professor**
**Department of Computer Science & Engineering**



## Implementation of TICK Stack in a dockerized environment
Tick stack is a platform of various types of tools that are open-source mainly used for collection, storing, graphing, and handling of time series data very easily. TICK stands for various components in the platform. It stands for **TELEGRAF INFLUXDB CHRONOGRAF KAPACITOR**. Briefly,
- data are sent to Telegraf.
- Then it is stored in the InfluxDB database. 
- Through a webpage, Chronograph will query the database. 
- Then, processing, monitoring, and raising alerts are done by Kapacitor.




### Description of docker-compose file
1. **Network**
It has a network entitled ```vcctestnetwork```.
2. **Environmental Variables**
We need to set up the certificate and private key for chronograf. They are set as:
```TLS_CERTIFICATE=/mycertis/dipinticertificate.pem```
```TLS_PRIVATE_KEY=/mycertis/umangkey.pem```
We also need to set the URL for InfluxDB configuration group. We can do it by  adding:
environment: 
      ```   KAPACITOR_INFLUXDB_0_URLS_0=http://influxdb:8086```
3. **Generation of certificate**
OpenSSL is used for generating the certificates for Chronograf. For this, RSA-2048 is used for encrypting securely.
```openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem```
4. Change the permission by using chmod command
```Sudo chmod -r 777 dipinticertificate.pem```
```Sudo chmod -r 777 umangkey.pem```
5. Move them into a new directory inside home
```Sudo mkdir /home/certificates```
```Sudo mv *.pem /home/certificates```
6. Run the docker-compose.yaml file
```docker-compose up -d```
7. Run ```https://localhost:8888``` in your favourite browser
8. **InfluxDB security**
After loading ```https://localhost:8888```, a page saying “Get Started” is opened. After clicking on that button, we need to setup the authentication for InfluxDB.
9. Replace the connection url with the IP address of InfluxDB container
10. Create a new username and password
11. Use the docker command to get the IP address of the InfluxDB container.
```docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' {containerID}```
Replace {containerID} with the containerID of InfluxDB container.


