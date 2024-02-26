# Librabry Management System

## Requirements

- A library database needs to store information pertaining to its users
(or customers), its workers, the physical locations of its branches, and
the media stored in those locations. We have decided to limit the
media to two types: books and videos.
- The library must keep track of the status of each media item: its
location, status, descriptive attributes, and cost for losses and late
returns. Books will be identified by their ISBN, and movies by their
title and year. In order to allow multiple copies of the same book or
video, each media item will have a unique ID number.
- Customers will provide their name, address, phone number, and date
of birth when signing up for a library card. They will then be assigned
a unique user name and ID number, plus a temporary password that
will have to be changed. Checkout operations will require a library
card, as will requests to put media on hold. Each library card will have
its own fines, but active fines on any of a customer's cards will prevent
the customer from using the library's services.
- The library will have branches in various physical locations. Branches
will be identified by name, and each branch will have an address and a
phone number associated with it. Additionally, a library branch will
store media and have employees.
- Employees will work at a specific branch of the library. They receive a
paycheck, but they can also have library cards; therefore, the same
information that is collected about customers should be collected about
employees.

- Functions for customers:
  - Log in
  - Search for media based on one or more of the following criteria:
    - type (book, video, or both)
    - title
    - author or director
    - year
  - Access their own account information:
    - Card number(s)
    - Fines
    - Media currently checked out
    - Media on hold
  - Put media on hold
  - Pay fines for lost or late items
  - Update personal information:
    - Phone numbers
    - Addresses
    - Passwords
    - 
- Functions for librarians are the same as the functions for customers plus the following:
    - Add customers
    - Add library cards and assign them to customers
    - Check out media
    - Manage and transfer media that is currently on hold
    - Handle returns
    - Modify customers' fines
    - Add media to the database
    - Remove media from the database
    - Receive payments from customers and update the customers'
    - nes
    - View all customer information except passwords

## Physical Databases Design

```sql
CREATE TABLE Status ( 
    code INTEGER, 
    description CHAR(30), 
    PRIMARY KEY (code) 
);
CREATE TABLE Media( 
    media_id INTEGER, 
    code INTEGER, 
    PRIMARY KEY (media_id),
    FOREIGN KEY (code) REFERENCES Status 
);
CREATE TABLE Book(
    ISBNCHAR(14), 
    title CHAR(128), 
    author CHAR(64),
    year INTEGER, 
    dewey INTEGER, 
    price REAL, 
    PRIMARY KEY (ISBN) 
);
CREATE TABLE BookMedia( 
    media_id INTEGER, 
    ISBN CHAR(14), 
    PRIMARY KEY (media_id),
    FOREIGN KEY (media_id) REFERENCES Media,
    FOREIGN KEY (ISBN) REFERENCES Book
);
CREATE TABLE Customer( 
    ID INTEGER, 
    name CHAR(64), 
    addr CHAR(256), 
    DOB CHAR(10),
    phone CHAR(30), 
    username CHAR(16), 
    password CHAR(32), 
    PRIMARY KEY (ID),
    UNIQUE (username) 
);
CREATE TABLE Card( 
    num INTEGER, 
    fines REAL, 
    ID INTEGER, 
    PRIMARY KEY (num),
    FOREIGN KEY (ID) REFERENCES Customer 
);
CREATE TABLE Checkout( 
    media_id INTEGER, 
    num INTEGER, 
    since CHAR(10),
    until CHAR(10), 
    PRIMARY KEY (media_id),
    FOREIGN KEY (media_id) REFERENCES Media,
    FOREIGN KEY (num) REFERENCES Card 
);
CREATE TABLE Location( 
    name CHAR(64), 
    addr CHAR(256), 
    phone CHAR(30),
    PRIMARY KEY (name) 
);
CREATE TABLE Hold( 
    media_id INTEGER, 
    num INTEGER, 
    name CHAR(64), 
    until CHAR(10),
    queue INTEGER, 
    PRIMARY KEY (media_id, num),
    FOREIGN KEY (name) REFERENCES Location,
    FOREIGN KEY (num) REFERENCES Card,
    FOREIGN KEY (media_id) REFERENCES Media 
);
CREATE TABLE Stored_In( 
    media_id INTEGER, name char(64), 
    PRIMARY KEY (media_id),
    FOREIGN KEY (media_id) REFERENCES Media ON DELETE CASCADE,
    FOREIGN KEY (name) REFERENCES Location 
);
CREATE TABLE Librarian( 
    eid INTEGER, 
    ID INTEGER NOT NULL, 
    Pay REAL,
    Loc_name CHAR(64) NOT NULL, 
    PRIMARY KEY (eid),
    FOREIGN KEY (ID) REFERENCES Customer ON DELETE CASCADE,
    FOREIGN KEY (Loc_name) REFERENCES Location(name) 
);
CREATE TABLE Video( 
    title CHAR(128), 
    year INTEGER, 
    director CHAR(64),
    rating REAL, 
    price REAL, 
    PRIMARY KEY (title, year) 
);
CREATE TABLE VideoMedia( 
    media_id INTEGER, 
    title CHAR(128), 
    year INTEGER,
    PRIMARY KEY (media_id), 
    FOREIGN KEY (media_id) REFERENCES Media,
    FOREIGN KEY (title, year) REFERENCES Video 
);
```