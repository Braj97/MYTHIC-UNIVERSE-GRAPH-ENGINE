# MYTHIC-UNIVERSE-GRAPH-ENGINE
Cypher
# CREATE NODES
// Delete existing graph (Careful in production)
MATCH (n)
DETACH DELETE n;
// Create Kingdoms
CREATE 
(k1:Kingdom {name:'Aryavarta', era:'Ancient', element:'Fire'}),
(k2:Kingdom {name:'Nagaloka', era:'Mythic', element:'Water'}),
(k3:Kingdom {name:'Suryadesh', era:'Golden Age', element:'Sun'});
// Create Gods
CREATE
(g1:God {name:'Agneya', power:95, domain:'Fire'}),
(g2:God {name:'Varunesh', power:92, domain:'Water'}),
(g3:God {name:'SuryaDev', power:99, domain:'Sun'});
// Create Warriors
CREATE
(w1:Warrior {name:'Ariv', strength:88, loyalty:90}),
(w2:Warrior {name:'Veerak', strength:91, loyalty:85}),
(w3:Warrior {name:'Nirvay', strength:85, loyalty:95});
// Create Weapons
CREATE
(we1:Weapon {name:'Flame Blade', damage:75}),
(we2:Weapon {name:'Ocean Spear', damage:70}),
(we3:Weapon {name:'Solar Bow', damage:80});
// Create Prophecy
CREATE
(p1:Prophecy {title:'Cosmic War', prediction:'Only balanced energy saves universe'});
# CREATE RELATIONSHIPS
// Gods rule Kingdoms
MATCH (g:God {name:'Agneya'}),(k:Kingdom {name:'Aryavarta'})
CREATE (g)-[:RULES]->(k);

MATCH (g:God {name:'Varunesh'}),(k:Kingdom {name:'Nagaloka'})
CREATE (g)-[:RULES]->(k);

MATCH (g:God {name:'SuryaDev'}),(k:Kingdom {name:'Suryadesh'})
CREATE (g)-[:RULES]->(k);
// Warriors belong to Kingdoms
MATCH (w:Warrior {name:'Ariv'}),(k:Kingdom {name:'Aryavarta'})
CREATE (w)-[:BELONGS_TO]->(k);

MATCH (w:Warrior {name:'Veerak'}),(k:Kingdom {name:'Nagaloka'})
CREATE (w)-[:BELONGS_TO]->(k);

MATCH (w:Warrior {name:'Nirvay'}),(k:Kingdom {name:'Suryadesh'})
CREATE (w)-[:BELONGS_TO]->(k);
// Warriors wield weapons
MATCH (w:Warrior {name:'Ariv'}),(we:Weapon {name:'Flame Blade'})
CREATE (w)-[:WIELDS]->(we);

MATCH (w:Warrior {name:'Veerak'}),(we:Weapon {name:'Ocean Spear'})
CREATE (w)-[:WIELDS]->(we);

MATCH (w:Warrior {name:'Nirvay'}),(we:Weapon {name:'Solar Bow'})
CREATE (w)-[:WIELDS]->(we);
// Gods empower Warriors
MATCH (g:God {name:'Agneya'}),(w:Warrior {name:'Ariv'})
CREATE (g)-[:EMPOWERS {energy:50}]->(w);

MATCH (g:God {name:'Varunesh'}),(w:Warrior {name:'Veerak'})
CREATE (g)-[:EMPOWERS {energy:45}]->(w);

MATCH (g:God {name:'SuryaDev'}),(w:Warrior {name:'Nirvay'})
CREATE (g)-[:EMPOWERS {energy:60}]->(w);
# ADVANCED COMPLEX QUERIES
FIND THE POWERFUL GOD.....
  MATCH (g:God)
RETURN g.name AS God, g.power AS Power
ORDER BY Power DESC
LIMIT 1;
CALCULATE TOTAL DAMAGE.....
MATCH (k:Kingdom)<-[:BELONGS_TO]-(w:Warrior)-[:WIELDS]->(we:Weapon)
RETURN k.name AS Kingdom, 
SUM(we.damage) AS TotalDamage
ORDER BY TotalDamage DESC;
ENERGY BALANCE....
MATCH (g:God)-[e:EMPOWERS]->(w:Warrior)
RETURN g.name AS God,
SUM(e.energy) AS TotalEnergyGiven;
PATH TRAVERSAL....
MATCH path = (g:God)-[:EMPOWERS]->(w:Warrior)-[:WIELDS]->(we:Weapon),
      (w)-[:BELONGS_TO]->(k:Kingdom)
RETURN g.name, w.name, we.name, k.name;
UNIVERSE SAFETY.....
MATCH (g:God)
WITH AVG(g.power) AS avgPower
MATCH (g2:God)
WHERE g2.power > avgPower
RETURN g2.name AS AboveAverageGods;
# CREATE COSMIC NETWORK
// Create Energy Flow Between Gods
MATCH (g1:God {name:'Agneya'}),(g2:God {name:'Varunesh'})
CREATE (g1)-[:RIVAL {tension:70}]->(g2);

MATCH (g2:God {name:'Varunesh'}),(g3:God {name:'SuryaDev'})
CREATE (g2)-[:ALLIANCE {trust:80}]->(g3);

MATCH (g3:God {name:'SuryaDev'}),(g1:God {name:'Agneya'})
CREATE (g3)-[:BALANCES {harmony:85}]->(g1);
# GRAPH VISUALISATION
MATCH (n)-[r]->(m)
RETURN n,r,m;



