# Code 1
class Room:
    """Represents a hotel room."""
    
    def __init__(self, room_number, room_type, price, available=True):
        # Initialize room details
        self.__room_number = room_number
        self.__room_type = room_type
        self.__price = price
        self.__available = available
    
    def is_available(self):
        """Checks if the room is available."""
        return self.__available
    
    def book_room(self):
        """Books the room if available."""
        if self.__available:
            self.__available = False  # Room is booked
            return True
        return False
    
    def get_price(self):
        """Returns the room price."""
        return self.__price
    
    def set_price(self, price):
        """Sets a new price for the room."""
        self.__price = price
    
    def get_room_number(self):
        """Returns the room number."""
        return self.__room_number
    
    def __str__(self):
        """Returns a string representation of the room."""
        return f"Room {self.__room_number}: {self.__room_type}, ${self.__price}/night, Available: {self.__available}"


class Guest:
    """Represents a guest."""
    
    def __init__(self, name, contact):
        # Initialize guest details
        self.__name = name
        self.__contact = contact
    
    def get_contact(self):
        """Returns the guest contact."""
        return self.__contact
    
    def set_contact(self, contact):
        """Sets a new contact for the guest."""
        self.__contact = contact
    
    def get_name(self):
        """Returns the guest name."""
        return self.__name
    
    def __str__(self):
        """Returns a string representation of the guest."""
        return f"Guest: {self.__name}, Contact: {self.__contact}"


class Booking:
    """Handles room reservations."""
    
    def __init__(self, booking_id, guest, room, check_in, check_out):
        # Initialize booking details
        self.__booking_id = booking_id
        self.__guest = guest
        self.__room = room
        self.__check_in = check_in
        self.__check_out = check_out
        
        # Check if the room is available before booking
        if room.is_available():
            self.__status = "Confirmed"  # Room booked successfully
            room.book_room()  # Book the room if available
        else:
            self.__status = "Failed"  # Room unavailable
    
    def get_status(self):
        """Returns the status of the booking."""
        return self.__status
    
    def get_booking_id(self):
        """Returns the booking ID."""
        return self.__booking_id
    
    def __str__(self):
        """Returns a string representation of the booking."""
        return f"Booking {self.__booking_id}: {self.__guest.get_name()}, Room: {self.__room.get_room_number()}, Status: {self.__status}"


class Payment:
    """Represents a payment transaction."""
    
    def __init__(self, payment_id, amount, method):
        # Initialize payment details
        self.__payment_id = payment_id
        self.__amount = amount
        self.__method = method
        self.__status = "Completed"  # Set payment status to completed
    
    def get_status(self):
        """Returns the payment status."""
        return self.__status
    
    def get_payment_id(self):
        """Returns the payment ID."""
        return self.__payment_id
    
    def __str__(self):
        """Returns a string representation of the payment."""
        return f"Payment {self.__payment_id}: ${self.__amount}, Method: {self.__method}, Status: {self.__status}"


# Test Case
if __name__ == "__main__":
    # Create test guest, room, booking, and payment
    guest1 = Guest("Alyazia", "alyazia@example.com")
    room1 = Room(101, "Single", 100.0)
    booking1 = Booking(1, guest1, room1, "2024-04-01", "2024-04-05")
    payment1 = Payment(1, room1.get_price() * 4, "Credit Card")
    
    # Print the results of the guest, room, booking, and payment
    print(guest1)
    print(room1)
    print(booking1)
    print(payment1)


output: 
Guest: Alyazia, Contact: alyazia@example.com
Room 101: Single, $100.0/night, Available: False
Booking 1: Alyazia, Room: 101, Status: Confirmed
Payment 1: $400.0, Method: Credit Card, Status: Completed


# Code 2;Checking
# Test Case 1: Testing Guest Creation
guest1 = Guest("Alyazia", "alyazia@example.com")
assert guest1.get_name() == "Alyazia"
assert guest1.get_contact() == "alyazia@example.com"
print("Test 1 Passed: Guest created successfully.")

# Test Case 2: Testing Room Availability
room1 = Room(101, "Single", 100.0)
assert room1.is_available() == True  # Initially, it should be available
room1.book_room()
assert room1.is_available() == False  # After booking, it should not be available
print("Test 2 Passed: Room availability checked.")

# Test Case 3: Reset Room Availability and Test Booking Confirmation
room1 = Room(101, "Single", 100.0)  # Resetting the room availability
guest1 = Guest("Alyazia", "alyazia@example.com")
booking1 = Booking(1, guest1, room1, "2024-04-01", "2024-04-05")
assert booking1.get_status() == "Confirmed"  # Booking should be confirmed
print("Test 3 Passed: Booking created successfully.")

# Test Case 4: Testing Payment Processing
payment1 = Payment(1, room1.get_price() * 4, "Credit Card")
assert payment1.get_status() == "Completed"  # Payment status should be 'Completed'
print("Test 4 Passed: Payment processed successfully.")

output: 
Test 1 Passed: Guest created successfully.
Test 2 Passed: Room availability checked.
Test 3 Passed: Booking created successfully.
Test 4 Passed: Payment processed successfully.


# Code 3;Grouping: 

code cell 1: 
class Room:
    """Represents a hotel room."""
    def __init__(self, number, room_type, price, available=True):
        """Creates a room."""
        self._number = number
        self._room_type = room_type
        self._price = price
        self._is_available = available

    def get_number(self): return self._number
    def get_room_type(self): return self._room_type
    def get_price(self): return self._price
    def is_available(self): return self._is_available
    def book(self):
        if self._is_available:
            self._is_available = False
            return True
        return False
    def set_availability(self, available): self._is_available = available
    def __str__(self): return f"Room {self._number} ({self._room_type}): ${self._price}, Available: {self._is_available}"


code cell 2: 
from typing import List

class Guest:
    """Represents a hotel guest."""
    def __init__(self, name, contact):
        """Creates a guest."""
        self._name = name
        self._contact = contact
        self._reservations: List['Booking'] = []

    def get_name(self): return self._name
    def get_contact(self): return self._contact
    def get_reservations(self): return self._reservations
    def add_reservation(self, booking: 'Booking'): self._reservations.append(booking)
    def __str__(self): return f"Guest: {self._name}, Contact: {self._contact}"


code cell 3:
from datetime import date
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from typing import Self

class Booking:
    """Represents a booking made by a guest."""
    def __init__(self, booking_id, guest, room, check_in, check_out):
        """Creates a booking."""
        self._booking_id = booking_id
        self._guest = guest
        self._room = room
        self._check_in = check_in
        self._check_out = check_out
        if room.book():
            self._status = "Confirmed"
        else:
            self._status = "Failed"
        guest.add_reservation(self)

    def get_booking_id(self): return self._booking_id
    def get_guest(self): return self._guest
    def get_room(self): return self._room
    def get_check_in(self): return self._check_in
    def get_check_out(self): return self._check_out
    def get_status(self): return self._status
    def set_status(self, status): self._status = status
    def __str__(self): return f"Booking {self._booking_id}: Guest {self._guest.get_name()}, Room {self._room.get_number()}, Status: {self._status}"


code cell 4:
class Payment:
    """Represents a payment for a booking."""
    def __init__(self, payment_id, amount, method):
        """Creates a payment."""
        self._payment_id = payment_id
        self._amount = amount
        self._method = method
        self._status = "Completed"

    def get_payment_id(self): return self._payment_id
    def get_amount(self): return self._amount
    def get_method(self): return self._method
    def get_status(self): return self._status
    def set_status(self, status): self._status = status
    def __str__(self): return f"Payment {self._payment_id}: ${self._amount}, Method: {self._method}, Status: {self._status}"


code cell 5:
class LoyaltyProgram:
    """Keeps track of guest loyalty points."""
    def __init__(self):
        """Starts the loyalty program with zero points."""
        self._points = 0

    def get_points(self): return self._points
    def add_points(self, amount): self._points += int(amount)
    def use_points(self, amount):
        if amount <= self._points:
            self._points -= amount
            return True
        return False
    def __str__(self): return f"Loyalty Points: {self._points}"


code cell 6:
class GuestServiceRequest:
    """Represents a request from a guest."""
    def __init__(self, request_id, guest, service_type):
        """Creates a service request."""
        self._request_id = request_id
        self._guest = guest
        self._service_type = service_type
        self._status = "Pending"

    def get_request_id(self): return self._request_id
    def get_guest(self): return self._guest
    def get_service_type(self): return self._service_type
    def get_status(self): return self._status
    def complete(self): self._status = "Completed"
    def __str__(self): return f"Request {self._request_id} for {self._guest.get_name()}: {self._service_type}, Status: {self._status}"


code cell 7:
class Feedback:
    """Represents feedback provided by a guest."""
    def __init__(self, feedback_id, guest, rating, comments=""):
        """Creates feedback."""
        self._feedback_id = feedback_id
        self._guest = guest
        self._rating = rating
        self._comments = comments

    def get_feedback_id(self): return self._feedback_id
    def get_guest(self): return self._guest
    def get_rating(self): return self._rating
    def get_comments(self): return self._comments
    def __str__(self): return f"Feedback {self._feedback_id} from {self._guest.get_name()}: Rating {self._rating}, Comments: {self._comments}"


code cell 8:
# Example Usage
if __name__ == "__main__":
    # Create some objects
    room1 = Room(101, "Single", 50.00)
    room2 = Room(202, "Double", 75.00)

    guest1 = Guest("Alyazia Alshamsi", "alyazia.shamsi@example.com")
    guest2 = Guest("Maryam Alblooshi", "maryam.blooshi@example.com")

    loyalty = LoyaltyProgram()
    loyalty.add_points(100)
    print(loyalty)

    booking1 = Booking(1, guest1, room1, date(2025, 4, 10), date(2025, 4, 15))
    booking2 = Booking(2, guest2, room2, date(2025, 5, 1), date(2025, 5, 5))

    payment1 = Payment(100, 250.00, "Credit Card")
    payment2 = Payment(101, 300.00, "Cash")

    request1 = GuestServiceRequest(1, guest1, "Room Service")
    request1.complete()

    feedback1 = Feedback(1, guest1, 5, "Great stay!")

    # Print info
    print("\n--- Hotel Info ---")
    print(room1)
    print(room2)
    print(guest1)
    print(guest2)
    print(booking1)
    print(booking2)
    print(payment1)
    print(payment2)
    print(request1)
    print(feedback1)


output: 
Loyalty Points: 100

--- Hotel Info ---
Room 101 (Single): $50.0, Available: False
Room 202 (Double): $75.0, Available: False
Guest: Alyazia Alshamsi, Contact: alyazia.shamsi@example.com
Guest: Maryam Alblooshi, Contact: maryam.blooshi@example.com
Booking 1: Guest Alyazia Alshamsi, Room 101, Status: Confirmed
Booking 2: Guest Maryam Alblooshi, Room 202, Status: Confirmed
Payment 100: $250.0, Method: Credit Card, Status: Completed
Payment 101: $300.0, Method: Cash, Status: Completed
Request 1 for Alyazia Alshamsi: Room Service, Status: Completed
Feedback 1 from Alyazia Alshamsi: Rating 5, Comments: Great stay!
