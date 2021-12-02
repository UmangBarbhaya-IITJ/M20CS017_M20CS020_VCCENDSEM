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

### Steps:
1. Take root access by the command:
```sudo -i ```
2. Change the directory to the folder
```cd /home/umang/Desktop/M20CS017_M20CS020_VCCENDSEM/```
3. Copy the docker-compose.yaml and remove the below 3 lines from the file 
*environment:*
      *- TLS_CERTIFICATE=/mycertis/dipinticertificate.pem*
      *- TLS_PRIVATE_KEY=/mycertis/umangkey.pem*
4. Create multiple containers:
```docker-compose up -d --build```
5. Go to link http://192.168.56.100:8000 in your host browser and Now the Tick stack platform will be opened and we can now create the influxdb creation
##### Now providing the https security
1. create a directory for certificate
```mkdir certificate```
2. Change directory to certificate
```cd certificate```
3.  Since we are not using any public domain we need to create the self-generated certificates which are done using the above command. It gives us a certificate and key file
```openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout umangkey.pem -out dipinticertificate.pem```
4. ```cd ..```
5. ```chmod -R 777 certificate```
6. Add the 3 lines in the docker-compose.yaml file which was removed above
*environment:*
     *- TLS_CERTIFICATE=/mycertis/dipinticertificate.pem*
      *- TLS_PRIVATE_KEY=/mycertis/umangkey.pem*

7. ``` docker-compose down```
8.  ```docker-compose up -d --build```
9.  Go to link https://192.168.56.100:8000 in your host browser, now you can see the ssl security has been added.
##### Data connection with kapacitor using telegraf.conf file
1. ```docker exec -it telegraf /bin/bash```
2. ```apt update```
3. ```apt install nano```
4. ```nano /etc/telegraf/telegraf.conf```
5. Uncomment and update the lines 
6. Reboot
7. Go to link https://192.168.56.100:8000 in your host browser
##### Create New connection with password protection
1. Modify the connection URL and add username and password for protection
2. Create the system dashboard
3. Add a Kapacitor connection with username and password
4. New InfluxDB Connection is created
5. We can see new host in the host list
6. Visualizing various parameters of the TICK stack host
##### Creating the alert
1. Go on the left bar for creating alert and select alert symbol and then click on manage Alert
2. Now click on the build alert rule on top right
3. Name the alert
4. Select the condition and its value
5. Select the filter Fields and save it. We have selected it as usage_idle, if CPU is idle more than 99% it will pop up the alert
6. Check the alert history to find the alert raised which can be used as an added security measure



 
