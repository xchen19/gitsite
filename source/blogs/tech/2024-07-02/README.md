# Parking lot

https://codemia.io/system-design/design-an-efficient-parking-lot-system/solutions/sbfgih/My-Solution-for-Design-an-Efficient-Parking-Lot-System-with-Score-910

## Database:
Reservation table:
reservation_ID (primary key)
lot_ID (foreign key)
user_ID (foreign key)
start_time
end_time
vehicle_type (foreign key)
payment_status (paid, unpaid, canceled)
completion_status (to be completed, fulfilled, canceled)


Vehicle_Type table:
vehicle_type_ID


Lot table:
lot_ID (primary key)
contact_info_ID (foreign key)
Lot_Space ID (foreign key to Lot_Space table)
capacities (foreign key to Lot_Capacity table)


Lot_Capacity table:
lot_space ID (primary key)
vehicle_type (foreign key to Vehicle_Type)
number of space


Contact_Info table:
contact_info_ID (primary key)
contact_type
contact_value


User table:
user_ID (primary key)
contact_info_ID (foreign key)
vehicles (foreign key to Vehicle table)


Vehicle table:
vehicle_ID (primary key)
vehicle_type (foreign key to Vehicle_Type table)


Transaction table:
transaction_ID (primary key)
user_ID (foreign key)
vehicle_ID (foreign key)
check_in_date_time 
check_out_date_time
