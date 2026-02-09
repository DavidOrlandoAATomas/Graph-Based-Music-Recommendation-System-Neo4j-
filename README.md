Graph-Based Music Recommendation System (Neo4j)

This project implements a music recommendation algorithm using graph databases, modeled and executed in Neo4j.
Instead of treating users and songs as rows in tables, the system models listening behavior, preferences, artists, and genres as a graph, allowing recommendations to emerge naturally from relationships.

This approach is inspired by real-world recommendation engines used by streaming platforms, where connections matter more than isolated attributes.

Core Idea

Traditional recommendation systems struggle to capture complex relationships.
Graphs do not.

By modeling users, music, artists, and genres as nodes, and interactions such as listening, liking, and following as relationships, we can:

Recommend music based on shared tastes

Discover hidden similarities between users

Graph Data Model
Node Labels
Label	Description
USER	Platform users
MUSIC	Songs
ARTIST	Artists / bands
GENRE	Musical genres
Relationship Types
Relationship	Direction	Purpose	Key Properties
LISTENED	USER → MUSIC	Listening history	count, lastPlayed, duration
LIKED	USER → MUSIC / ARTIST	Explicit preference	timestamp, weight
FOLLOWS	USER → ARTIST	Artist subscription	since, strength
CREATED_BY	MUSIC → ARTIST	Song authorship	—
IN_GENRE	MUSIC → GENRE	Music classification	—

Recommendation Strategies
Content-Based Recommendation (Genre Similarity)

Recommend songs from the same genre as music the user already liked.

MATCH (u:USER)-[:LIKED]->(:MUSIC)-[:IN_GENRE]->(g:GENRE)<-[:IN_GENRE]-(rec:MUSIC)
WHERE NOT (u)-[:LISTENED]->(rec)
RETURN u.name AS user, rec.titule AS recommendation
ORDER BY user;

Collaborative Filtering (User Similarity)

Recommend music liked by similar users.

MATCH (u:USER)-[:LIKED]->(:MUSIC)<-[:LIKED]-(other:USER)-[:LIKED]->(rec:MUSIC)
WHERE u <> other
  AND NOT (u)-[:LISTENED]->(rec)
RETURN u.name AS user, rec.titule AS recommendation, COUNT(*) AS score
ORDER BY score DESC;
