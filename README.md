# IPS-Application
In-Door Positioning System application aims to help users locate and navigate inside the building

## Repositories description
1. IPS-MAP: Map service which handles map related functionality
    - Map gRPC: Map service main handler
    - Presence gRPC: Presence service handle recording online user
2. IPS-RSSI: RSSI related functionality
    - RSSI gRPC: Handle saving RSSI data for model training and predict user location
3. IPS-BFF: Backend for Frontend where transform in coming RestAPI request to gRPC request
    - BFF Api: Act as proxy between front-end and back-end
    - Realtime API: Broadcasting online users to all admin account
5. IPS-Protobuf: Protobuf definition repository where each gRPC service will refer their service definition from using the git submodule.


## High level design
![image](https://github.com/RyuChk/IPS-Application/assets/89927721/a128eb00-80cf-4ba1-8d20-0adb020e3595)
Link : https://whimsical.com/high-level-design-diagram-AP7JVpNYEM7iW2jWEGkKRs 

## Flow Design
Link : https://whimsical.com/flow-diagram-Qxd2K5HtiKeHwsAzKumH2q

### User get coordinate
The user gets current coordinates to display on to map. During this process, we will update user presence to presence service which will be saved to the Redis database.

1. Application(Client) starts web socket connection with BFF(Backend for frontend) and constantly polls requests to predict the current location from RSSI information.
2. BFF will get user information using Access Token that is bound in the request header.
3. BFF passes user information and requests to RSSI service.
4. RSSI Service will then send the position to model service to predict user location.
5. After the model service predicts user location RSSI receives the result and saves it to Mongodb to later be analyzed.
6. RSSI will send the user location to the presence service to be stored in Redis for an online heartbeat.
7. Return prediction result to the Application

<img width="812" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/2b0fda8d-c50c-46d7-bf28-dc388025c6fa">

### Online user flow
This flow is to broadcast an online user list to all admin users for display of other user locations in the application.

1. When the user gets the location RSSI service will send the result to the presence service that will save the information with timestamp to Redis. The TTL of the keys will be set slightly greater than the polling rate of get coordinate.
2. The application will create an SSE connection with real-time service to listen to online user broadcasting.
3. Realtime service will constantly poll online users from the presence service where the presence service will scan the Redis database for existing records.
4. 
<img width="613" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/7565492d-edd0-46fe-a934-7394ca4c7593">


### Store RSSI data for model training flow
1. The experiment application sent RSSI information to BFF(Backend for frontend)
2. BFF will send all the requests to the RSSI service
3. RSSI service will preprocess data (filter and outlined data)
4. RSSI service will save raw request information and transfer information into the database.

<img width="514" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/3d0757e1-f7ee-440f-995f-292ebfc2909c">

### Get registered floor and floor map URL flow
The application will request floor information from the server to display. After the application gets registered floor list application will then request the server for the current selected floor map to display.

1. Application will request a floor list using the building as a parameter and Access Token as an authentication header
2. BFF will get user information and bind the user info into the request before sending the request to the map service.
3. The map service will check if the information has already been stored in Redis. If not get data from MongoDB and then save it to Redis as a cache.
4. Return the floor list to the application
> **Noted:** (This logic applies as same as getting the floor map URL)

<img width="658" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/221501f8-9c3a-4375-8ccd-c596e536bfe1">



