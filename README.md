# IPS-Application
In-Door Positioning System application aims to help users locate and navigate inside the building

## Repositories description
1. IPS-MAP: Map service which handles map related functionality
    - Map gRPC: Map service main handler
    - User Tracker gRPC: Track online user
2. IPS-RSSI: RSSI related functionality
    - Data Collection gRPC: Collect data for train model
    - User Manager gRPC: Manage user functionality
3. IPS-BFF: Backend for Frontend where transform in coming RestAPI request to gRPC request
    - BFF Api: Act as proxy between front-end and back-end
5. IPS-Protobuf: Protobuf definition repository where each gRPC service will refer their service definition from using the git submodule.


## High level design
<img width="1211" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/d678ed68-e052-45cb-96b9-73b53e1836a1">
Link : https://whimsical.com/high-level-design-diagram-AP7JVpNYEM7iW2jWEGkKRs 

## Flow Design
Link : https://whimsical.com/flow-diagram-Qxd2K5HtiKeHwsAzKumH2q

### User get coordinate
The user gets current coordinates to display on to map. During this process, we will update user presence to presence service which will be saved to the Redis database.

1. Application(Client) starts web socket connection with BFF(Backend for frontend) and constantly polls requests to predict the current location from RSSI information.
2. BFF will get user information using Access Token that is bound in the request header.
3. BFF passes user information and requests to User manager service.
4. User manager service will then send the position to prediction service to predict user location.
5. After the prediction service predicts and response location user manager service will then saves result to Mongodb to later be analyzed.
6. User manager will send the user location to the user tracking service to be stored in Redis for an user online tracking.
7. Return prediction result to the Application

<img width="554" alt="image" src="https://github.com/RyuChk/IPS-Application/assets/89927721/76e15d47-4a77-482a-ad7d-f3f854226687">

### Online user flow
This flow is to broadcast an online user list to all admin users for display of other user locations in the application.

1. When the user gets the location the user manager service will send the result to the user tracking service and save the information with timestamp to Redis. The TTL of the keys will be set slightly greater than the polling rate of get coordinate.
2. The application will set polling interval to fetch online user from server.

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



