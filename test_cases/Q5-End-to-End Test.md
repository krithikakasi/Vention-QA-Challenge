## End-to-End Test 

## Create a weapon that follows the mapping (weapon_qa.png)
#### QA stick weapon is added to 003_weapons.js as API to create new weapon is not exposed.
```
exports.seed = async function (knex) {
  await knex("weapons_materials").del();
  await knex("weapons").del();
  return await Promise.all([
    await knex("weapons").insert([
      { id: 1, name: "Excalibur", power_level: 0, status: "new" },
      { id: 2, name: "Magic Staff", power_level: 0, status: "new" },
      { id: 3, name: "Axe", power_level: 0, status: "new" },
      { id: 4, name: "QA Stick", power_level: 0, status: "new" }
     
    ]),
    await knex("weapons_materials").insert([
      { id: 1, weapon_id: 1, material_id: 1 },
      { id: 2, weapon_id: 1, material_id: 6 },
      { id: 3, weapon_id: 1, material_id: 9 },
      { id: 4, weapon_id: 1, material_id: 12 },
      { id: 5, weapon_id: 2, material_id: 6 },
      { id: 6, weapon_id: 3, material_id: 9 },
      { id: 7, weapon_id: 3, material_id: 12 },
      { id: 8, weapon_id: 4, material_id: 1},
      { id: 9, weapon_id: 4, material_id: 9}
    ])
  ]);
};
```

## Calculate the power level of the weapon using a recursive query

1. Created the following enteries in the database table materials and table compositions.


```
-- INIT database
CREATE TABLE materials (
  ID INT ,
  Name VARCHAR(100),
  base_power INT
);

INSERT INTO materials(ID, Name, base_power) VALUES (9,'Flint',90)
INSERT INTO materials(ID, Name, base_power) VALUES (10,'0bsidian',130)
INSERT INTO materials(ID, Name, base_power) VALUES (11,'Brass',220)
INSERT INTO materials(ID, Name, base_power) VALUES (1,'Iron',100)
INSERT INTO materials(ID, Name, base_power) VALUES (2,'Steel',20)
INSERT INTO materials(ID, Name, base_power) VALUES (3,'Bronze',60)
INSERT INTO materials(ID, Name, base_power) VALUES (4,'Copper',10)
INSERT INTO materials(ID, Name, base_power) VALUES (5,'Wood',30)
INSERT INTO materials(ID, Name, base_power) VALUES (12,'Silver',300)

CREATE TABLE compositions (
  ParentID INT ,
  materialID INT,
  qty INT
);

INSERT INTO compositions(ParentID, materialID, qty) VALUES (9,10,5)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (10,11,10)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (1,2,2)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (2,3,5)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (2,4,5)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (3,5,5)
INSERT INTO compositions(ParentID, materialID, qty) VALUES (3,12,1)

```

2. Query the DB to do a join on both the tables and get the required data to calculate the power level for the new weapon QA Stick.

```
-- QUERY database
SELECT ID, base_power, compositions.materialID, compositions.qty 
FROM materials
FULL OUTER JOIN compositions
ON materials.ID = compositions.ParentID;

Output is the following where each material ID shows the parent material ID and the QTY required.:

ID	base_power	materialID	qty
9	90	    	10			5
10	130			11			10
11	220			NULL		NULL
1	100			2			2
2	20			3			5
2	20			4			5
3	60			5			5
3	60			12			1
4	10			NULL		NULL
5	30			NULL		NULL
12	300			NULL		NULL

```
3. Calculate power level on QA stick

  > > ID 9 ➡️ 90 + 5*(130 + 10*220) = **11,740**
  > > > 90 = base_power WHERE ID=9
  > > >
  > > > 5 = qty WHERE ID=9
  > > > 
  > > > 130 = base_power WHERE ID=10 
  > > >  
  > > > 10 = SELECT QTY WHERE ID=10
  > > >
  > > > 220 = base_power WHERE ID=11 



  > > ID 1 ➡️ 100 + 2*(20 + 5*60 + 5*30 + 1*300 + 5*10)= **1740**
  > > > 100 = base_power WHERE ID=1
  > > >
  > > > 2= qty WHERE ID=1
  > > > 
  > > > 20 = base_power WHERE ID=2 
  > > >  
  > > > 5 = SELECT QTY WHERE ID=2 
  > > >
  > > >60 = base_power WHERE ID=3 
  > > >
  > > >5 = SELECT QTY WHERE ID=3
  > > >
  > > >30= base_power WHERE ID=5
  > > >
  > > >5 = SELECT QTY WHERE ID=1
  > > >
  > > >300= base_power WHERE ID=12
  > > >
  > > >5 = SELECT QTY WHERE ID=2 AND MaterialID=4
  > > >
  > > >10= base_power WHERE ID=4

  > Power Level of QA Stick = ***13,480***



