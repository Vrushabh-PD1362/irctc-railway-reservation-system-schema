IRCTC (Railway Booking) : Database Schema Design


Tables

Stations
station_id BIGINT PK AUTO_INCREMENT
code VARCHAR(40) NOT NULL UNIQUE
name VARCHAR(40) NOT NULL
city_id BIGINT FK(Cities.city_id)
latitude DECIMAL(9, 6)
longitude DECIMAL(9, 6)


States
state_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Cities
city_id BIGINT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
state_id INT FK(States.state_id)


Trains
train_id BIGINT PK AUTO_INCREMENT
train_number VARCHAR(40) NOT NULL UNIQUE
name VARCHAR(40) NOT NULL
operator_id INT FK(Operators.operator_id)
route_id  INT FK(Routes.route_id)
service_type_id INT FK(Service_Types.service_type_id)


Operators
operator_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Service_Types
service_type_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Routes
route_id BIGINT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
Route_Stations
route_station_id BIGINT PK AUTO_INCREMENT
route_id BIGINT FK(Routes.route_id)
station_id BIGINT FK(Stations.station_id)
station_order INT NOT NULL
arrival_time TIME
departure_time TIME
arrival_day_offsets INT
departure_day_offsets INT 
UNIQUE(route_id, station_id)


Train_Runs
train_run_id BIGINT PK AUTO_INCREMENT
train_id BIGINT NOT NULL FK(Trains.train_id)
service_date DATE NOT NULL
UNIQUE(train_id, service_date)


Train_Calendar_Exceptions
train_calendar_exception_id INT PK AUTO_INCREMENT
train_id BIGINT NOT NULL FK(Trains.train_id)
service_date DATE NOT NULL
exception ENUM(added, cancelled) NOT NULL
UNIQUE(train_id, service_date)


Coach_Types
coach_type_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
sequence_order INT NOT NULL


Train_Run_Coaches
train_run_coaches_id BIGINT PK AUTO_INCREMENT
train_run_id BIGINT FK(Train_Runs.train_run_id)
coach_type_id INT FK(Coach_Types.coach_type_id )
coach_sequence INT NOT NULL


Coach_Berths
coach_berth_id BIGINT PK AUTO_INCREMENT
train_run_coaches_id BIGINT NOT NULL FK(Train_Run_Coaches.train_run_coaches_id )
berth_position_id INT NOT NULL FK(Berth_Positions.berth_position_id)
berth_number VARCHAR(40) NOT NULL
quota_id INT FK(Quotas.quota_id)
layout_metadata JSON  
UNIQUE(train_run_coaches_id, berth_number)
Berth_Positions
berth_position_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL UNIQUE


Quotas
quota_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL UNIQUE
rules TEXT


Distance_Slabs
distance_slab_id INT PK AUTO_INCREMENT
min_distance INT NOT NULL
max_distance INT NOT NULL
fare DECIMAL(10, 2) NOT NULL


Classes
class_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
fare DECIMAL(10, 2) NOT NULL


Concessions
concession_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
discount_type ENUM(‘percentage’, ‘fixed’)
discount_value DECIMAL(10, 2) NOT NULL


Booking_Statuses
booking_status_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


ID_Proof_Types
id_proof_type_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL UNIQUE


Bookings
booking_id BIGINT PK AUTO_INCREMENT
prn_number VARCHAR(40) NOT NULL UNIQUE
train_run_id BIGINT NOT NULL FK(Train_Runs.train_run_id)
booking_date DATETIME NOT NULL
booking_status_id INT NOT NULL FK(Booking_Statuses.booking_status_id)


Passengers
passenger_id BIGINT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL
age INT NOT NULL
gender VARCHAR(40) NOT NULL
berth_preference_id INT FK(Berth_Positions.berth_position_id)
id_proof_type_id INT FK(ID_Proof_Types.id_proof_type_id)


Passenger_Bookings
passenger_booking_id BIGINT PK AUTO_INCREMENT
passenger_id BIGINT NOT NULL FK(Passengers.passenger_id)
booking_id BIGINT NOT NULL FK(Bookings.booking_id)
original_station_id BIGINT NOT NULL FK(Stations.station_id)
destination_station_id BIGINT NOT NULL FK(Stations.station_id)
boarding_date DATE NOT NULL
concession_id INT FK(Concessions.concession_id)
quota_id INT FK(Quotas.quota_id)
distance_slab_id INT FK(Distance_Slabs.distance_slab_id)
class_id INT NOT NULL FK(Classes.class_id)
distance_fare DECIMAL(10, 2) NOT NULL
class_fare DECIMAL(10, 2) NOT NULL
other_charges DECIMAL(10, 2) NOT NULL
concession_amount DECIMAL(10, 2) NOT NULL
total_fare DECIMAL(10, 2) NOT NULL	
created_at DATETIME 
booking_status_id INT NOT NULL FK(Booking_Statuses.booking_status_id)
booking_position_number INT NULL
UNIQUE(passenger_id, booking_id)


Seat_Allocations
seat_allocation_id BIGINT PK AUTO_INCREMENT
passenger_booking_id BIGINT NOT NULL FK(Passenger_Bookings.passenger_booking_id)
train_run_coach_id BIGINT NOT NULL FK(Train_Run_Coaches.train_run_coach_id )
coach_berth_id BIGINT NOT NULL FK(Coach_Berths.coach_berth_id )
allocated_at DATETIME NOT NULL
UNIQUE(passenger_booking_id)
UNIQUE(train_run_coach_id, coach_berth_id)


Payment_Statuses
payment_status_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Refund_Statuses
refund_status_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Payment_Modes
payment_mode_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Payments
payment_id BIGINT PK AUTO_INCREMENT
transaction_id VARCHAR(40) NOT NULL UNIQUE
payment_mode_id INT NOT NULL FK(Payment_Modes.payment_mode_id)
payment_status_id INT NOT NULL FK(Payment_Statuses.payment_status_id)
amount DECIMAL(10, 2) NOT NULL
created_at DATETIME NOT NULL


Booking_Payments
booking_payment_id BIGINT PK AUTO_INCREMENT
booking_id BIGINT NOT NULL FK(Bookings.booking_id)
payment_id BIGINT NOT NULL FK(Payments.payment_id)
UNIQUE(payment_id)


Booking_Cancellations
booking_cancellation_id BIGINT PK AUTO_INCREMENT
passenger_booking_id BIGINT NOT NULL FK(Passenger_Bookings.passenger_booking_id)
reason VARCHAR(400) 
cancelled_at DATETIME NOT NULL
cancellation_charge_id INT NOT NULL FK(Cancellation_Charges.cancellation_charge_id)
cancellation_charge DECIMAL(10, 2) NOT NULL


Cancellation_Charges
cancellation_charge_id INT PK AUTO_INCREMENT
min_time_to_departure INT NOT NULL
max_time_to_departure INT NOT NULL
charges DECIMAL(10, 2) NOT NULL


Payment_Refunds
payment_refund_id BIGINT PK AUTO_INCREMENT
booking_cancellation_id BIGINT NOT NULL FK(Booking_Cancellations.booking_cancellation_id)
payment_id BIGINT NOT NULL FK(Payments.payment_id)
refund_transaction_id VARCHAR(40) NOT NULL
refund_status_id INT NOT NULL FK(Refund_Statuses.refund_status_id)
refund_amount DECIMAL(10, 2) NOT NULL
created_at DATETIME NOT NULL
processed_at DATETIME


Boarding_Point_Changes
boarding_point_change_id BIGINT PK AUTO_INCREMENT
passenger_booking_id BIGINT NOT NULL FK(Passenger_Bookings.passenger_booking_id)
boarding_station_id BIGINT NOT NULL FK(Stations.station_id)
added_at DATETIME
status TINYINT


Chart_Statuses
chart_status_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL UNIQUE


Chart_Preparations
chart_preparation_id BIGINT PK AUTO_INCREMENT
train_run_id BIGINT NOT NULL FK(Train_Runs.train_run_id)
cuttoff_at DATETIME NOT NULL
prepared_at DATETIME 
allocation_locked_at DATETIME 
chart_status_id INT NOT NULL FK(Chart_Statuses.chart_status_id)


Coach_Charts
coach_chart_id BIGINT PK AUTO_INCREMENT
chart_preparation_id BIGINT NOT NULL FK(Chart_Preparations.chart_preparation_id )
train_run_coach_id BIGINT NOT NULL FK(Train_Run_Coaches.train_run_coach_id  )
UNIQUE (chart_preparation_id, train_run_coach_id)


Chart_Passengers
chart_passenger_id BIGINT PK AUTO_INCREMENT
coach_chart_id  BIGINT NOT NULL FK(Coach_Charts.coach_chart_id)
passenger_booking_id  BIGINT NOT NULL FK(Passenger_Bookings.passenger_booking_id)
coach_berth_id  BIGINT FK(Coach_Berths.coach_berth_id)
UNIQUE (coach_chart_id, passenger_booking_id)


Operation_Disruption_Types
operation_disruption_type_id INT PK AUTO_INCREMENT
name VARCHAR(40) NOT NULL UNIQUE


Operation_Disruptions
operation_disruption_id BIGINT PK AUTO_INCREMENT
operation_disruption_type_id INT NOT NULL FK(Operation_Disruption_Types.operation_disruption_type_id)
train_run_id BIGINT NOT NULL FK(Train_Runs.train_run_id)
status ENUM(cancel, reschedule) NOT NULL


Affected_Bookings
affected_booking_id  BIGINT PK AUTO_INCREMENT
passenger_booking_id  BIGINT NOT NULL FK(Passenger_Bookings.passenger_booking_id)
operation_disruption_id  BIGINT NOT NULL FK(Operation_Disruptions.operation_disruption_id )
resolution_type ENUM(‘refunded’, ‘rebooked’) NOT NULL
UNIQUE (passenger_booking_id, operation_disruption_id) 


Users
user_id INT PRIMARY KEY AUTO_INCREMENT
name VARCHAR(40) NOT NULL


Audit_Logs
audit_log_id BIGINT PRIMARY KEY AUTO_INCREMENT
entity_id VARCHAR(40) NOT NULL
entity_type_id VARCHAR(40) NOT NULL
action_type ENUM(‘insert’, ‘update’, ‘delete’) NOT NULL
old_values JSON,
new_values JSON,
timeline DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
operator_id INT NOT NULL FK(Users.user_id)



Cardinality of Relations : 

Between Stations and Cities => m : 1
Between Cities and States => m : 1
Between Trains and Operators => m : 1
Between Trains and Service_Types => m : 1
Between Route_Stations and Routes => m : 1
Between Route_Stations and Stations => m : 1
Between Train_Runs and Trains => m : 1
Between Train_Calendar_Exceptions and Trains => m : 1
Between Train_Run_Coaches and Train_Runs => m : 1
Between Train_Run_Coaches and Coach_Types => m : 1
Between Coach_Berths and Train_Run_Coaches => m : 1
Between Coach_Berths and Berth_Positions => m : 1
Between Bookings and Train_Runs => m : 1
Between Bookings and Booking_Statuses => m : 1
Between Passengers and Berth_Positions => m : 1
Between Passengers and ID_Proof_Types => m : 1
Between Passenger_Bookings and Passengers => m : 1
Between Passenger_Bookings and Bookings => m : 1
Between Passenger_Bookings and Stations => m : 1
Between Passenger_Bookings and Concessions => m : 1
Between Passenger_Bookings and Quotas => m : 1
Between Passenger_Bookings and Distance_Slabs => m : 1
Between Passenger_Bookings and Classes => m : 1
Between Passenger_Bookings and Booking_Statuses => m : 1
Between Payments and Payment_Modes => m : 1
Between Payments and Payment_Statuses => m : 1
Between Booking_Payments and Bookings => m : 1
Between Booking_Payments and Payments => m : 1
Between Booking_Cancellations and Passenger_Bookings => m : 1
Between Booking_Cancellations and Cancellation_Charges => m : 1
Between Payment_Refunds and Booking_Cancellations => m : 1
Between Payment_Refunds and Payments => m : 1
Between Payment_Refunds and Refund_Statuses => m : 1
Between Boarding_Point_Changes and Passenger_Bookings => m : 1
Between Boarding_Point_Changes and Stations => m : 1
Between Chart_Preparations and Train_Runs => m : 1
Between Chart_Preparations and Chart_Statuses => m : 1
Between Coach_Charts and Chart_Preparations => m : 1
Between Coach_Charts and Train_Run_Coaches => m : 1
Between Chart_Passengers and Coach_Charts => m : 1
Between Chart_Passengers and Coach_Berths => m : 1
Between Operation_Disruptions and Coach_Berths => m : 1
Between Operation_Disruptions and Train_Runs => m : 1
Between Affected_Bookings and Passenger_Bookings => m : 1
Between Affected_Bookings and Operation_Disruptions => m : 1
Between Audit_Logs and Users => m : 1

Between Stations and Routes => m : m





