
## API Test Cases

#### TC Environment


|TC Name|GET all materials|
|---|---|
| TC ID  |  1 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/material (null Payload) |
| Expected Result| Returns HTTP code 200 OK <br> Returns all the materials from the db in Material array|
             
|TC Name|GET all material with unsupported endpoint|
|---|---|
| TC ID  |  2 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/materials Payload: null |
| Expected Result| Returns HTTP code 404 Not found without any errors in the backed.Check logs to ensure to exceptions are thrown.|

|TC Name|GET material by ID|
|---|---|
| TC ID  |  3 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/material/2 Payload: null|
| Expected Result| Returns HTTP code 200 OK.Returns the Material with id 2 as:  {id: 2, name: 'Steel', base_power: 20, qty: 1, deleted_at: null } |

|TC Name|GET material for non-existing ID in the DB|
|---|---|
| TC ID  |  4 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/material/200 Payload: null |
| Expected Result| Returns HTTP code 404 Not Found with message "Material ID does not exist|

|TC Name|PUT to update an existing material|
|---|---|
| TC ID  |  5 |
| Pre-conditions  |  Ensure host is running |
| Test Step 1 |   PUT /api/material/2 Payload: { base_level: 40, qty: 2 } |
| Expected Result 1| Returns HTTP code 200 OK implying material is updated successfully.The response should have the updated materials and weapons that were updated.Response:{ updatedMaterials:MaterialMaterial[], updatedWeapons: Weapon[] }|
| Test Step 2 |   GET /api/material/2 Payload:  null  to check if the updated was successful|
| Expected Result 2| Returns HTTP code 200 OK with the updated Material: {id: 2, name: 'Steel', base_power_level: 40, qty: 2, deleted_at: null }| 


|TC Name|PUT to update a non-existing material|
|---|---|
| TC ID  |  6 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   PUT /api/material/201 Payload: { power_level: 40, qty: 2 } |
| Expected Result| Returns HTTP code 404 Not Found with message "Material ID does not exist|


|TC Name|DELETE material|
|---|---|
| TC ID  |  7 |
| Pre-conditions  |  Ensure host is running |
| Test Step 1 |   DELETE /api/material/12 Payload: null |
| Expected Result 1| Returns HTTP code 204 No Content implying that deletion was successful. Response body should have { deletedMaterials: Material[], brokenWeapons: Weapon[] }|
| Test Step 2 |   GET /api/material/12 Payload: null to ensure the deleted material is not returned|
| Expected Result 2| Returns HTTP code 404 Not found with the message with message "Material ID does not exist" |
 

|TC Name|DELETE material for non-existing ID in the DB|
|---|---|
| TC ID  |  8 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   DELETE /api/material/200 Payload: null |
| Expected Result| Returns HTTP code 404 Not Found with message "Material ID does not exist|
 

|TC Name|POST to create a new material|
|---|---|
| TC ID  |  9 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   POST /api/material  Payload: [{{ id: 14, name: 'Agate', power_level: 500, qty: 500, deleted_at: null }}] |
| Expected Result| Returns HTTP code 405 Method Not Allowed as creating new material is not supported|

|TC Name|GET /api/weapon/:id/maxBuildQuantity|
|---|---|
| TC ID  | 10 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/weapon/1/maxBuildQuantity  Payload: null |
| Expected Result| Returns HTTP code 200 OK with response { weapon: Weapon, maxBuildQty: number }|

|TC Name|GET /api/weapon/:id/maxBuildQuantity with non existing weapon id|
|---|---|
| TC ID  | 11 |
| Pre-conditions  |  Ensure host is running |
| Test Step  |   GET /api/weapon/100/maxBuildQuantity  Payload: null |
| Expected Result| Returns HTTP 404 NotFound with the message "Weapon Id does not exist" |


