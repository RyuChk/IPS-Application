<a name="readme-top"></a>

<br />
<div align="center">
  <a href="https://github.com/RyuChk/IPS-Application">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">IPS-Application</h3>

  <p align="center">
    In-Door Positioning System
    <br />
    <a href="https://drive.google.com/file/d/1moKA6AbRSryunTpYLjnbz9Fe9mnRBJc3/view?usp=drive_link"><strong>Our Report »</strong></a>
    <br />
    <br />
    <a href="https://youtu.be/0v2ftHYCSK0">View Demo</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#contact">Contact</a></li>

  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## About The Project

<!-- [![Product Name Screen Shot][product-screenshot]](https://example.com) -->

Smartphone-based Indoor Localization System are made to help the users know their location in the building. Aimed to be able to estimate floors and the position in the building and show it in the application.

Objective:

- Develop an indoor positioning system utilizing Android phones and RSSI from WiFi Access Points for precise tracking.
- Enable multi-user functionality.
- Identify the user's floor and room accurately.
- Maintain an error margin of less than 5 meters.
- Ensure an efficient system architecture and flow.

The position has been calculated from the RSSI value from each of the access points in the building, calculated with the classification model.
The model also made from the experiment that we have to conduct it ourselves.

Applications:

1. Main IPS Application

   This application is for tracking users in target building having two main features.
   MyMap is to track user in the building on the map, it will identify building, floor and label of the location.
   Overwatch (For Admin Only) is to track all active users in the selected building and floor.

2. Experimental Application

   This application is for collecting data in experiment phase used to train model (will be used in prediction service) having three modes.
   Single Mode is to collect one record of RSSI data from target accesss points.
   Continuous Mode is to keep collect data until user want to stop.
   Custom Mode is to keep collect data until it reach record-limit which can be define by user.

Services:

1. User Manager Service

   This service handles user-related functionality such as predicting user location. For
   predicting user location, the user manager service needs to communicate with the
   prediction service which runs a machine learning model for predicting the user
   location.

2. Backend For Frontend Service

   This service'sobjective is to validate the user access, retrieveuser information using
   the access token, and transform the Rest API request to gRPC request.

3. User Tracking Service

   User tracking service will handle retrieving currentonline users and updating online
   user status. After the user manager service receives the prediction result from the
   prediction service, the user manager services will send an online heartbeat to the
   user tracking service to update the user's presence.

4. Map Service

   Map service handles map-related functionality like the list of building registered in
   the system until the list of room in each building registered in the floor that user
   is eligible to receive. This service also includes floor, room and building registration.

5. Data Collection Service

   The main objective of data collection service is to process the raw information
   from experiment applications into useable data for training the model.

6. Prediction Service

   This service is hosting the machine learning model and will predict user’s coordinate from incoming getting coordinate request and communicate with the user manager service.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

Our major programming languages/frameworks/libraries.

- [![Golang][Go]][Golang-url]
- [![gRPC][gRPC]][gRPC-url]
- [![Flutter][Flutter]][Flutter-url]
- [![Dart][Dart]][Dart-url]
- [![Redis][Redis]][Redis-url]
- [![MongoDB][MongoDB]][MongoDB-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

This is an instructions on setting up our project.

<!-- ### Prerequisites

List things you need to use the software and how to install them.

- npm
  ```sh
  npm install npm@latest -g
  ``` -->

### Installation

Installing and setting up application.

1. Clone the repo
   ```sh
   git clone https://github.com/RyuChk/IPS-Application.git
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Contact

- Naphat Chaisang - puninw1500@gmail.com
- Thanwa Chokporntaveesuk - rewapk@gmail.com

Project Link: [https://github.com/RyuChk/IPS-Application.git](https://github.com/RyuChk/IPS-Application.git)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->

<!-- ## Acknowledgments -->

[Go]: https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white
[Golang-url]: https://go.dev/
[gRPC]: https://img.shields.io/badge/gRPC-00ADD8?style=for-the-badge&logo=grpc&logoColor=white
[gRPC-url]: https://grpc.io/
[Flutter]: https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white
[Flutter-url]: https://flutter.dev/
[Dart]: https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white
[Dart-url]: https://dart.dev/
[Redis]: https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white
[Redis-url]: https://redis.io/
[MongoDB]: https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white
[MongoDB-url]: https://www.mongodb.com/
