# ATS-T

Versatile, modular, flexible Attendance Taking System for Telegram, bot. The bot is available on Telegram as ATS-TBot

The goals of this system are to:

- Facilitate easy attendance taking for large groups of personnel on a daily basis
- Track absence, absence reasons and verification of absence
  - Verification of absence can be achieved through pictorial means (e.g user snaps a picture of their approved leave application)
- Easy to use administration interface directly through Telegram
  - No need to connect to servers, minimal IT knowledge required to monitor this system
- Powerful location capability to make sure that all personnel are accounted for in a geofence
- Hassle free deployment of server assets (hopefully a serverless system, but if not possible, a self contained system will do)

The secondary goals of this system are:

## Features

### Uncomplete features

- Interface for users
  - Enrollment
    - Enroll with a single key given out by admins, enter their full name and await admin approval
    - Admin approves user to be enrolled into group
  - Clocking in
    - Clock in or out with a single button or command
    - Bot performs verification of the user's location
  - Absenteesm
    - Declare temporary statuses for absenteesm such as reporting sick
    - Submit photographic records for verification of absenteesm to change a temporary status into a permanent status (e.g. flow: MC unverified -> An admin checks and approves MC -> MC verified. The same can apply for leave forms which has been approved in person)
  - Submit special records
    - Additional photographic record checking for special records, similar to MC checking, such as checks related to the current Covid-19 pandemic (e.g. "Have you been tested? Please upload your test results as a photo")
  
- Interface for admins
  - Enrollment for admins
    - System would be in "unsafe mode" when there are zero admins in the database
    - The first admin can enroll themselves by connecting to the bot, becoming the owner of the bot
  - Deploying enrollment for users
    - Generate pre-shared key to enroll users into a given group: Enroll users based on a preset secret code given out by administrators for a certain group of people, to prevent accidental enrollment.
    - Approve enrollment of user individually (perhaps auto-notify the admin that there's a new user who enrolled?)
  - Approval mechanism
    - Approve absenteesm records (e.g. MC, leave form)
    - Approve special records (e.g. Covid-19 swab test results)
  - Reporting mechanism
    - Request for daily report for a particular day
    - Identify those who report sick
  
- Database
  - Table for storing details of a group
  - Table for storing all users and their associated groups
  - Table for storing all attendance records with timestamps, locations, and status of each user
  - Table for connecting to an external storage medium (local storage, S3 etc to retrieve or upload images)

### Complete features

WIP

### Database schema

#### Table: groups

- secret_code (Indicates the pre-generated secret code for the group): TEXT
- group_name (Name of the group): TEXT

#### Table: users

This spec is subject to change with Telegram's API

- phone_number (Phone number of personnel): TEXT
- full_name (Full name of personnel): TEXT
- admin (whether the individual is an admin in a given group): INTEGER (boolean)
- excluded: INTEGER (boolean)

#### Table: attn_record

- id: autoincrement ID
- phone_number: TEXT (foreign key to phone_number in users table)
- type (Type of attendance): INT (enum for PRESENT, RS, MC_UNVER, MC_VER, VL_UNVER, VL_VER, UNKNOWN, AWOL)

#### Table: spec_record

- id: autoincrement ID
- phone_number: TEXT (foreign key to phone_number in users table)
