# IPS-Application
In-Door Positioning System application aim to help user locate and navigat inside the building

## Repositories description
1. IPS-MAP: Map service which handle map related functionality
    - Map gRPC: Map service main handler
    - Presence gRPC: Presence service handle recording online user
2. IPS-RSSI: RSSI related functionality
    - RSSI gRPC: Handle saving RSSI data for model training and predict user location
3. IPS-BFF: Backend for Frontend where transform in comming RestAPI request to gRPC request
4. IPS-Protobuf: Protobuf definition repository where each gRPC service will refer their service definition from using gitsubmodule.


## High level design
![image](https://github.com/RyuChk/IPS-Application/assets/89927721/a128eb00-80cf-4ba1-8d20-0adb020e3595)
Link : https://whimsical.com/high-level-design-diagram-AP7JVpNYEM7iW2jWEGkKRs 

## Flow Design

### User get coordinate
User getting current coordinate to display on to map. During this process we will update user presence to presence service which will be save to redis database.

1. Application(Client) start web socket connection with BFF(Backend for frontend) and constantly polling request to predict current location from RSSI information.
2. BFF will get user information using Access Token that binded in request header.
3. BFF pass user information and it request to RSSI service.
4. RSSI Service will then sent the position to Map service to predict user location.
5. After model service predict user location RSSI recieved the result and save to mongodb to later be analyzed.
6. RSSI will sent user location to presence service to be store in redis for online heartbeat.
7. Return back prediction result to Application

<img width="812" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/2b0fda8d-c50c-46d7-bf28-dc388025c6fa">

### Store RSSI data for model training flow
1. Experiment application sent RSSI information to BFF(Backend for frontend)
2. BFF will sent all the request to RSSI service
3. RSSI service will preprocess data (filter and outlined data)
4. RSSI service will save raw request information and transfromed information into database.

<img width="514" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/3d0757e1-f7ee-440f-995f-292ebfc2909c">

### Get registered floor and floor map URL flow
Application will request floor information from server to display. After application get register floor list application will then request server for current selected floor map to display.

1. Application will request floor list using building as parameter and Access Token as authentication header
2. BFF will get user information and bind the user info into the request before sent request to map service.
3. Map service will check if the information has been already store in redis. If not get data from MongoDB and then save to redis as cache.
4. Return floor list to application
> **Noted :** (This logic apply as same as getting floor map url)

<img width="658" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/221501f8-9c3a-4375-8ccd-c596e536bfe1">



