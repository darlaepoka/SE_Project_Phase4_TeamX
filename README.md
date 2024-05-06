# SE_Project_Phase4_TeamX
# Team Name: Dental Clinic Management System (DCMS)
# Project Title: Dental Care ProTech
### Introduction to Testing:
Software testing is the process of evaluating a software component or system to identify defects or bugs. It involves executing the software under specific conditions and comparing its expected behavior with its actual behavior. Testing is crucial in software development to ensure reliability, correctness, and quality of the final product.
### Purpose of Testing:
The purpose of testing is to identify defects early in the development process and verify that software components perform as intended. By thoroughly testing each component, developers can ensure that the software meets the requirements and functions correctly in various scenarios.
### Focus on Testing a Single Component:
The chosen component for testing is the Appointment class within the appointmentsTest.php file. This class encapsulates the functionality related to managing appointments in the dental clinic system.
Testing this component is crucial due to the following reasons:

Role: The Appointment class handles the creation, editing, deletion, and retrieval of appointment data from the database. As appointments are a core feature of the dental clinic system, ensuring the correctness of this component is vital for the overall system's functionality.

Impact on the System: Incorrectly managed appointments can lead to scheduling conflicts, loss of data, or disruption in patient care. Therefore, thorough testing of this component can prevent such issues and ensure the reliability of the system.
### Preparing Test Cases:
The test code: appointmentsTest.php
<?php

use PHPUnit\Framework\TestCase;
class appointmentsTest extends TestCase
{
    private $conn;
public function setUp(): void
{
    // Create database connection
    $this->conn = new mysqli("localhost", "enton", "@Enton@2334", "dental_clinic");
    if ($this->conn->connect_error) {
        die("Connection failed: ". $this->conn->connect_error);
    }
}
    public function tearDown(): void
    {
        // Close the database connection
        $this->conn->close();
    }
    public function testCreateAppointment()
    {
        $appointment = new Appointment($this->conn);
        $result = $appointment->create("John Doe", "johndoe@example.com", "Dr. Smith", "Check-up", "2023-03-15", "10:00");
        $this->assertTrue($result);
    }
    public function testEditAppointment()
    {
        $appointment = new Appointment($this->conn);
        $result = $appointment->edit(7, "Jane Doe", "janedoe@example.com", "Dr. Johnson", "Follow-up", "2023-03-16", "11:00");
        $this->assertTrue($result);
    }
    public function testDeleteAppointment()
    {
        $appointment = new Appointment($this->conn);
        $result = $appointment->delete(8);
        $this->assertTrue($result);
    }
    public function testGetAppointments()
    {
        $appointment = new Appointment($this->conn);
        $result = $appointment->getAll();
        $this->assertNotEmpty($result);
    }
}
class Appointment
{
    private $conn;
    public function __construct($conn)
    {
        $this->conn = $conn;
    }
    public function create($name, $email, $doctor, $service, $date, $time)
    {
        $query = "INSERT INTO appointments (name, email, doctor, service, appointment_date, appointment_time) VALUES (?,?,?,?,?,?)";
        $stmt = $this->conn->prepare($query);
        $stmt->bind_param("ssssss", $name, $email, $doctor, $service, $date, $time);
        $stmt->execute();
        return $stmt->affected_rows > 0;
    }
    public function edit($id, $name, $email, $doctor, $service, $date, $time)
    {
        $query = "UPDATE appointments SET name =?, email =?, doctor =?, service =?, appointment_date =?, appointment_time =? WHERE id =?";
        $stmt = $this->conn->prepare($query);
        $stmt->bind_param("ssssssi", $name, $email, $doctor, $service, $date, $time, $id);
        $stmt->execute();
        return $stmt->affected_rows > 0;
    }
    public function delete($id)
    {
        $query = "DELETE FROM appointments WHERE id =?";
        $stmt = $this->conn->prepare($query);
        $stmt->bind_param("i", $id);
        $stmt->execute();
        return $stmt->affected_rows > 0;
    }
    public function getAll()
    {
        $query = "SELECT * FROM appointments";
        $result = $this->conn->query($query);
        return $result->fetch_all(MYSQLI_ASSOC);
    }
}

setUp() and tearDown() Methods:

The setUp() method is executed before each test method in the class. It establishes a database connection using mysqli and sets it to $this->conn.
The tearDown() method is executed after each test method. It closes the database connection to ensure clean-up.

testCreateAppointment() Method:
This method tests the create() function of the Appointment class.
It creates a new instance of the Appointment class, passing the database connection ($this->conn) as a parameter.

It calls the create() method with sample data for a new appointment (name, email, doctor, service, date, time).

The return value of the create() method is stored in $result.

The assertion assertTrue($result) verifies that the appointment creation was successful.

The purpose of this test is to ensure that the create() method of the Appointment class properly inserts a new appointment into the database. If the method works as expected, it should return true, indicating successful insertion.
### Choosing Testing Frameworks:
For PHP-based testing, PHPUnit is a popular choice for unit testing. It provides a robust framework for writing and executing tests. Additionally, PHPUnit supports various assertion methods for validating test outcomes. To set up PHPUnit for testing, we install PHPUnit via Composer: composer require --dev phpunit/phpunit
### Test Coverage:
High test coverage is essential to ensure thorough testing of software. It helps identify areas of the codebase that lack proper testing and reduces the likelihood of undiscovered bugs.
Achieving high test coverage involves:
Writing comprehensive test cases that cover different functionalities and scenarios.
Ensuring that tests exercise various paths through the code, including edge cases and error conditions.
Regularly reviewing and updating tests to accommodate changes in the codebase.
