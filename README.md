# Capstone: Restaurant Reservation System API
Backend and API of the Restaurant Reservation System for storing and delivering a restaurant's reservations and tables data.

## Links:
- [App Demo](https://capstone-restaurant-reservations-system.vercel.app/)
- [App Documentation](https://github.com/angelalouh/capstone-restaurant-reservations-system)

## Technology
### Built with:
- Node.js
- Express server framework
- PostgreSQL database
- Knex.js for query building

## API Documentation
All requests return JSON response. All post requests require application/json body, and return JSON response. 

### Endpoints for reservations:
**Get Reservations:** If the URL is `/dashboard?date=2035-12-30`, send GET to `/reservations?date=2035-12-30`
- Requests all reservations for the date specified in the query string.
- Successful GET request will return an array of JSON objects representing the saved reservations for a particular date. Each reservation object contains the following fields:
  - `reservation_id`: integer
  - `first_name`: string
  - `last_name`: string
  - `mobile_number`: string
  - `reservation_date`: string
  - `reservation_time`: string
  - `status`: string
  - `people`: integer
  - `created_at`: date
  - `updated_at`: date

**Get Reservation by ID:** GET to `/reservations/:reservation_id`
- Request a specific reservation by its reservation_id.
- Successful GET request will return a JSON object representing the reservation that will be edited. 

**Create New Reservation:** POST to `/reservations`
- POST a single JSON object
  - POST body must contain `first_name`, `last_name`, `mobile_number`, `reservation_date`, `reservation_time`, `people` values.
    - Note: `people` cannot be zero.
    - Note: `reservation_date` and `reservation_time` cannot be in the past.
    - Note: `reservation_date` cannot be on a Tuesday. The restaurant is closed on Tuesdays.
    - Note: `reservation_time` must be within the eligible timeframe of 10:30 AM to 9:30 PM.
- New reservations will have a default status of `booked`.
- Successful POST request will return JSON of the posted object.

**Update Reservation:** PUT to `/reservations/:reservation_id`
- PUT request will be sent with a single JSON object containing the updated reservation information.
  - Note: Only reservations with a status of `booked` can be edited.
- Successful PUT request will return JSON of the updated reservation object.

**Update Reservation Status:** PUT to `/reservations/:reservation_id/status`
- PUT request will be sent with a single JSON object containing the new status of the reservation.
  - Note: Reservations with a status of `finished` cannot be updated.
- Successful PUT request will return JSON of the updated reservation object with the new status.

### Endpoints for tables:
**Create New Table:** POST to `/tables`
- POST a single JSON object.
  - POST body must contain `table_name` and `capacity`.
    - Note: `table_name` must be at least 2 characters long.
    - Note: `capacity` must be at least 1.
- Successful POST request will return JSON of the posted object.

**Get Tables:** GET to `/tables`
- Request all existing tables.
- Successful GET request will return an array of JSON objects representing the saved tables. Each table object contains the following fields:
  - `table_id`: integer
  - `table_name`: string
  - `capacity`: integer
  - `reservation_id`: integer
  - `created_at`: date
  - `updated_at`: date
- Note: `reservation_id` will be `null` unless a reservation has been assigned to the table.

**Update Table:** PUT to `/tables/:table_id/seat`
- PUT request will be sent with a single JSON object containing the `reservation_id` of the reservation that will be seated at that table.
  - Note: A reservation cannot be seated at a table with a `capacity` that is less than the number of `people` in the reservation.
  - Note: A reservation cannot be seated at a table that is already occupied.
- Successful PUT request will return JSON of the updated table object with the `reservation_id` of the reservation that was assigned to the table.

**Delete Table Assignment:** DELETE to `/tables/:table_id/seat`
- DELETE request will be sent without a request body.
  - Note: The request will not succeed if the table is not occupied.
- Successful DELETE request will remove the `reservation_id` assigned to the table by setting the value to `null`. 

### Installation:
1. Fork and clone this repository.
2. Run `cp ./.env.sample ./.env`.
3. Update the `.env` file with the connection URL's to your database instance.
4. Run `npm install` to install project dependencies.
5. Run `npm run start:dev` to start your server in development mode.
