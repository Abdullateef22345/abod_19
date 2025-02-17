# abod_19
CREATE DATABASE LittleLemonBooking;
USE LittleLemonBooking;

CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    phone_number VARCHAR(15)
);

CREATE TABLE Rooms (
    room_id INT AUTO_INCREMENT PRIMARY KEY,
    room_type VARCHAR(50),
    price DECIMAL(10, 2)
);

CREATE TABLE Bookings (
    booking_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    room_id INT,
    booking_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (room_id) REFERENCES Rooms(room_id)
);
pip install mysql-connector-python
import mysql.connector

# Connect to the database
db_connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="LittleLemonBooking"
)

cursor = db_connection.cursor()

# Example: Insert a new customer
cursor.execute("INSERT INTO Customers (first_name, last_name, email, phone_number) VALUES ('John', 'Doe', 'johndoe@email.com', '123-456-7890')")
db_connection.commit()

# Close the connection
cursor.close()
db_connection.close()
DELIMITER //
CREATE PROCEDURE AddBooking(
    IN customer_id INT, 
    IN room_id INT, 
    IN booking_date DATE
)
BEGIN
    INSERT INTO Bookings (customer_id, room_id, booking_date)
    VALUES (customer_id, room_id, booking_date);
END;
//
DELIMITER ;
cursor.callproc("AddBooking", [1, 2, '2025-03-10'])
db_connection.commit()
