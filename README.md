# Welcome to Flights Service

## Project Setup
- clone the project on your local
- Execute `npm install` on the same path as of your root directory of teh downloaded project
- Create a `.env` file in the root directory and add the following environment variable
    - `PORT=3000`
- Inside the `src/config` folder create a new file `config.json` and then add the following piece of json

```
{
  "development": {
    "username": <YOUR_DB_LOGIN_NAME>,
    "password": <YOUR_DB_PASSWORD>,
    "database": "Flights_Search_DB_DEV",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}

```
- Once you've added your db config as listed above, go to the src folder from your terminal 
and execute `npx sequelize db:create`
and then execute

`npx sequelize db:migrate`
```

## DB Design
  - Airplane Table
  - Flight
  - Airport
  - City 

  - A flight belongs to an airplane but one airplane can be used in multiple flights
  - A city has many airports but one airport belongs to a city
  - One airport can have many flights, but a flight belongs to one airport



## Tables

### City -> id, name, created_at, updated_at
### Airport -> id, name, address, city_id, created_at, updated_at
    Relationship -> City has many airports and Airport belongs to a city (one to many)

```
npx sequelize model:generate --name Airport --attributes name:String,address:String,cityId:integer
```
## Description

The flight service is a backend project that I have created and implemented micrservices architecture, it is a comprehensive system that manages various aspects of a flight service. 

This Service has several models airplane, airport, city, flight, and seat models for the efficient and organized operation of the flight service.

- The **airplane** model includes essential information such as the **airplane's model Number**, **seating capacity**.

- The **airport** model contains information about each airport, such as its **name**, **location**, **City code**, **address**. This model enables efficient handling of airport-related data like flight scheduling, arrivals, and departures.

- The **city** model represents the cities or locations connected by the flight service. It stores information about each **city name**.

- The **flight** model serves as the core entity within the system, capturing details about individual flights. It contains information such as the **flight number**, **airplaneId**, **departureAirportId** and **arrivalAirportId**, scheduled **departure** and **arrival times**, **price**, **totalSeats** and **boardingGate** assigned to the flight.

- The **seat** model represents the seating arrangements within each airplane. It includes information about **airplaneId**, **row**, **col**, **seatType** (e.g., economy, business, first class). It enables efficient allocation of seats during the booking process.

By integrating these models within our flight service backend project, we have established a robust system that enables effective management of airplanes, airports, cities, flights, and seats.


## API Reference

#### Airplane endpoint

```http
  /api/v1/airplanes
```

| Parameter | Method   | Body | Description                          |
| :-------- | :------- |:------- |:-------------------------         |
| `/` | `GET` |         |Get all Airplanes     |
| `/{id}` | `GET` |         |Get Airplane by ID     |
| `/` | `POST` | `modelNumber`, `capacity`       |Create New Airplanes     |
| `/{id}` | `DELETE` |         |Delete Airplane by ID     |
| `/{id}` | `PATCH` |  `capacity`       |Update Airplane     |

#### City endpoint

```http
  /api/v1/cities
```

| Parameter | Method   | Body | Description                          |
| :-------- | :------- |:------- |:-------------------------         |
| `/` | `GET` |         |Get all Cities     |
| `/` | `POST` | `name`       |Create New City     |
| `/{id}` | `DELETE` |         |Delete City by ID     |
| `/{id}` | `PATCH` |  `name`       |Update Airplane     |

#### Airport endpoint

```http
  /api/v1/airports
```

| Parameter | Method   | Body | Description                          |
| :-------- | :------- |:------- |:-------------------------         |
| `/` | `GET` |         |Get all Airports     |
| `/{id}` | `GET` |         |Get Airport by ID     |
| `/` | `POST` | `name`, `cityId`, `code`       |Create New Airport     |
| `/{id}` | `DELETE` |         |Delete Airport by ID     |
| `/{id}` | `PATCH` |  `name`, `cityId`, `code` (Min one field required)      |Update Airplane     |


#### Flight endpoint

```http
  /api/v1/flights
```

| Parameter | Method   | Body | Description                          |
| :-------- | :------- |:------- |:-------------------------         |
| `/*[?trip=MUM-DEL]` | `GET` |         |Get all Flights     |
| `/` | `POST` | `flightNumber`, `airplaneId`, `departureAirportId`, `arrivalAirportId`, `arrivalTime`, `departureTime`, `price`, `totalSeats`, `boardingGate(optional)`       |Create New City     |
| `/{id}` | `DELETE` |         |Delete Flights by ID     |
| `/{id}/seats` | `PATCH` |  `seats`, `*[dec]`       |Update Seats     |


**Note**
1. `[ ]` -> Inside this is Optional
2. if Dec will not be given in the parameter it will decrease the seat by default. if 
given dec = false it will increase the seats.

3. By Default `\` will fetch all the flights.

Query parameter 

- `trips` -> `MUM-DEL`
- `price` -> `1200-7000` or `2000`
- `travellers` -> `2`
- `tripDate` -> `02-12-23`
- `sort` -> `departureTime_ASC, price_DESC`