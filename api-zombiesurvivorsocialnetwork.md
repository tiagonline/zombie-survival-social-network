FORMAT: 1A
HOST: http://polls.apiblueprint.org/

# zombie-survivor-social-network - Alex Roberto Corrêa

This document aims to define the technical specifications and criteria
required to interact with the Zombie Survivor Social Network application.

All the API communication is performed via HTTP-based REST pattern and
using JSON data format.

## Survivor Collection

###### Survivor Parameter: 

| Field                   | Type     | Obligatory  | Description |
|:------------------------|:---------|:-----------:|:------------|
| _id                     | string   | N           | Single code generated by the application (output only).
| name                    | string   | Y           | Survivor's name.
| age                     | number   | Y           | Survivor's age.
| gender                  | string   | Y           | Survivor's gender (Male or Female).
| location                | object   | Y           | Last location notified about the survivor.
| location.latitude       | string   | Y           | Exact latitude notified about the survivor.
| location.longitude      | string   | Y           | Exact longitude notified about the survivor.
| inventory               | object   | N           | Survivor's inventory of items.
| inventory.water         | number   | N           | Number of water's recipient in the survivor's inventory.
| inventory.food          | number   | N           | Number of food's recipient in the survivor's inventory.
| inventory.medication    | number   | N           | Number of medication's things in the survivor's inventory.
| inventory.ammunition    | number   | N           | Number of ammunition's cartridge in the survivor's inventory.
| infected                | boolean  | Y           | Set if survivor is infected (true), or not (false).
| numInfectedNotification | number   | N           | Number of notifications' survivor. It starts with 0, but when it gets to 3, the survivor is set to infected.

## /api/survivor

### Create a New Suvivor [POST]

You may create your own survivors using this action. It takes a JSON
object containing a survivor object.

+ Request (application/json)

        {
            "name": "Eric Soto",
            "age": 36,
            "gender": "Male",
            "location": {
                "latitude": 10.22261,
                "longitude": -15.91212
            },
            "inventory": {
                "water": 9,
                "food": 5,
                "medication": 3,
                "ammunition": 5
            },
            "infected": false,
            "numInfectedNotification": 0
        }

+ Response 201 (application/json)

    + Headers

    + Body

            {
                "name": "Eric Soto",
                "age": 36,
                "gender": "Male",
                "location": {
                    "latitude": 10.22261,
                    "longitude": -15.91212
                },
                "inventory": {
                    "water": 9,
                    "food": 5,
                    "medication": 3,
                    "ammunition": 5
                },
                "infected": false,
                "numInfectedNotification": 0,
                "_id": "5a6dfd2e1e5e79254fb3ee46"
            }

## /api/survivor/:survivor-id/location

### Update survivor location [PATCH]

You may update the survivor location. To do this, you need to provide the latitude and longitude (they both are required).

+ Request (application/json)

        {
            "latitude": -19.8157,
            "longitude": -43.9542
        }

+ Response 200 (application/json)

    + Headers
    
    
## /api/survivor/:survivor-id/denunciation/infected

### Notify survivor as infected [PATCH]

To notify a survivor as infected, you can to this operation. If a survivor is notified 3 times,he/she become infected.

+ Request (application/json)

        {
        }

+ Response 200 (application/json)

    + Headers
    

## /api/survivor/:first-survivor-id/:second-survivor-id/trade

### Trade supplies between survivors [PATCH]

To trade supplies between survivors, two survivors have to exist and they don't have to be infected. So, you can trade water, food, medication and
ammunition, but a survivor wants some supply, the another survivor have to have that amount of it. Next, the trade is made respecting a rule of points:
each supply has a amount of points like the table below:

| Item         | Points   |
|:-------------|:---------|
| 1 Water      | 4 points |
| 1 Food       | 3 points |
| 1 Medication | 2 points |
| 1 Ammunition | 1 point  |

For example, if survivor 1 has 1 water and survivor 2 has 2 medications, they can trade because (1 X 4) is equal (2 X 2), and so on.

To do this, you need patch an object with two parameters: giveToMe and receiveToYou. So, giveToMe has the supplies (name and quantity) that the
survivor 1 will receive from survivor 2 and survivor 2 will give them to survivor 1. The parameter receiveToYou is exactly the inverse.

+ Request (application/json)

        {
            "giveToMe": {
                "ammunition": 6,
                "food": 2
            },
            "receiveToYou": {
                "water": 1,
                "medication": 4
            }
        }
        
+ Response 200 (application/json)

    + Headers
    

## /api/survivors/reports

### Report about survivors [GET]

To get the complete report about survivors, you may do this operation. This reports the percentage of infected survivors,
percentage of non-infected survivors, average amount of each kind of resource by survivor and points lost because of infected survivor and
the total of points lost. A least a survivor have to exist to generate the report.

+ Response 200 (text/plain)

        Reports:
        
        Percentage of infected survivors: 50%
        
        Percentage of non-infected survivors: 50%
        
        Average amount of each kind of resource by survivor:
        4 ammunition per survivor
        6.75 food per survivor
        1.75 water per survivor
        4.25 medication per survivor
        
        Points lost because of infected survivor
        ammunition: 9
        food: 48
        water: 16
        medication: 12
        Total of points lost: 85
