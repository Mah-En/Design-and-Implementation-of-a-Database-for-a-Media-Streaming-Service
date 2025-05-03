# DataBase Project - Phase 1

## Mahla Entezari

## Overview
In this project, I designed and built a database for a **Media Streaming Service** .
The database stores critical information, such as 
* User profiles
* Subscription details
* Payment histories
* Personalized watch lists
* ..

It also manages media assets, including movies and series with multiple episodes while tracking their storage locations and associated production companies.
The system ensures that all data is efficiently organized to support user activities like
* Leaving comments
* Rating media
* Curating a watch list for future viewing
* ..


## Specifications:
- Each user can subscribe to the service for a period of time. The start
and end date of their subscription (including the subscription tier) must
be stored.
- The users’ payment history, including the related subscription and
transaction details, must be stored as well.
- A media entity can be a movie or a series, and it includes a genre, a
production year, an average rating score, a director, a list of actors, a list of producers, and a list of comments left by users.
- Users can leave multiple comments for a media entity but can rate each
item once.
- A series consists of multiple episodes. Each movie or episode of a
series must have a storage location, which stores the corresponding
server and path of the media file.
- Each movie or series must belong to a production company. Each
company's details, such as its name, year of establishment, and contact
information, must be stored in the database.
- Each user has a ‘Watch Later’ list to which specific movies or episodes of a series can be added.



## Requirements Analysis

1. Identifying the project’s main entities and their attributes based on the specifications provided:
**Entities :**
   - User
     - `UserID` -> To identify each user
     - `Username` -> For user to login
     - `Password` -> For authentication
     - `Email` -> For login and communications
     - `SubscriptionTier` -> `Free`, `Premium`, ..
     - `SubscriptionStartDate` -> Start of subscription
     - `SubscriptionEndDate` -> End of subscription
     
     I chose User because we want to store some details of users.
   - Payment
     - `PaymentID` -> To idetify each payment
     - `UserID` -> To show each user this payment belongs to
     - `AmountPaid` -> How much it paid
     - `SubscriptionID` -> For which subscription this payment was made
    
     I chose payment because we want to store some details of payments.
   - Media
     - `MediaID` -> To identify
     - `Title` -> For easy searching
     - `Type` -> `Series` or `Movie`
     - `Genre` -> Search for arbitrary genre
     - `ProductionYear` 
     - `AverageRatingScore` 
     - `Director`
     - `Actors`
     - `Producers`
     - `Comments`
      
      I chose Media because we want to store some details of Media.
   - Company
     - `CompanyID` -> To identify
     - `Name`
     - `YearOfEstablishment`
     - `PhoneNumber` -> Contact Information
     - `Location` -> Contact Information
     - `Email` -> Contact Information
    
      I chose Company for linking to Media.
   - Comment
     - `CommentID`
     - `UserID`
     - `MediaID`
     - `Text`
     
      I chose Comment to link with Users and Media.
   - Episode
     - `EpisodeID`
     - `SeriesID`
     - `Title`
     - `StorageID`
     
      I chose Episode to link with Series.
    - Movie 
      - `MovieID`
      - `StorageID`
      - `MediaID`
      - `Title`
      
      I chose Movie to link with Media.
    
    - Series
      - `SeriesID`
      - `MediaID`
      - `Title`
      
      I chose Series to link with Media and Episodes.
    - StorageLocation
      - `StorageID`
      - `Server`
      - `Path`
   
      I chose StorageLocation to store Server and Path of the media file.
   - Rating
     - `RatingID`
     - `UserID`
     - `MediaID`
     - `Score`
	
	I chose Rating to store rating scores.
   - WatchLater
     - `ListID`
     - `UserID`
     - `MediaID`

	I chose WatchLater to store list of medias for watching later.
   

2. The relationships and constraints required to model a streaming service accurately:

# Relationships

| Entity1          | Entity2             | Type             | Explanation                                                                 | Constraints                                         | Details                                                                                      |
|-------------------|---------------------|------------------|-----------------------------------------------------------------------------|----------------------------------------------------|---------------------------------------------------------------------------------------------|
| User             | Payment             | One-to-Many      | A user can make multiple payments.                                          | `UserID` in **Payment** references `UserID` in **User**. | Payments track user subscription fees and transactions over time.                           |
| User             | Comment             | One-to-Many      | A user can leave multiple comments on media items.                          | `UserID` in **Comment** references `UserID` in **User**. | Comments store user feedback or reviews for movies and series.                              |
| User             | Rating              | One-to-Many      | A user can rate multiple media items, but only once per item.               | Composite unique key `(UserID, MediaID)` in **Rating**. | Ratings are numerical scores (e.g., 1–5) left by users for media items.                     |
| User             | Watch Later         | Many-to-Many     | A user can save multiple media items to their "Watch Later" list.           | `UserID` and `MediaID` as a composite key in **WatchLater**. | This feature allows users to save movies or episodes for future viewing.                    |
| Media            | Production Company  | Many-to-One      | Many media items can belong to a single production company.                 | `ProductionCompanyID` in **Media** references `CompanyID` in **ProductionCompany**. | Media items like movies and series are associated with the company that produced them.      |
| Series           | Episode             | One-to-Many      | A series can have multiple episodes.                                        | `SeriesID` in **Episode** references `SeriesID` in **Series**. | Series represent collections of episodes, with each episode uniquely identified.            |
| Movie/Episode    | StorageLocation     | One-to-One       | Each movie or episode has one unique storage location.                      | `StorageID` in **Movie/Episode** references `StorageID` in **StorageLocation**. | Storage location tracks the server and file path for streaming or retrieval.                |
| Media            | Comment             | One-to-Many      | Each media item can have multiple comments.                                 | `MediaID` in **Comment** references `MediaID` in **Media**. | Users leave reviews and comments on media items like movies or episodes.                    |
| Media            | Rating              | One-to-Many      | Each media item can have multiple ratings.                                  | `MediaID` in **Rating** references `MediaID` in **Media**. | Ratings enable calculating the average rating score for movies or series.                   |

---

# Attribute Constraints Table

| **Table**          | **Attribute**           | **Data Type**         | **Constraints**                                                                 |
|---------------------|-------------------------|-----------------------|-------------------------------------------------------------------------------|
| **User**           | `UserID`                | `INT`                 | Primary Key, Not Null                                                         |
|                    | `Username`              | `VARCHAR(255)`        | Unique, Not Null                                                              |
|                    | `Password`              | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `Email`                 | `VARCHAR(255)`        | Unique, Not Null                                                              |
|                    | `SubscriptionTier`      | `VARCHAR(50)`         | Check: Values in ('Free', 'Premium'), Default: 'Free', Not Null               |
|                    | `SubscriptionStartDate` | `DATE`                | Not Null                                                                      |
|                    | `SubscriptionEndDate`   | `DATE`                | Not Null, Check: `SubscriptionStartDate` < `SubscriptionEndDate`              |
| **Payment**        | `PaymentID`             | `INT`                 | Primary Key, Not Null                                                         |
|                    | `UserID`                | `INT`                 | Foreign Key (User), Not Null                                                  |
|                    | `AmountPaid`            | `DECIMAL(10,2)`       | Not Null, Check: `AmountPaid > 0`                                             |
|                    | `SubscriptionID`        | `INT`                 | Foreign Key (Subscription), Not Null                                          |
| **Media**          | `MediaID`               | `INT`                 | Primary Key, Not Null                                                         |
|                    | `Title`                 | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `Type`                  | `VARCHAR(50)`         | Check: Values in ('Series', 'Movie'), Not Null                                |
|                    | `Genre`                 | `VARCHAR(100)`        | Not Null                                                                      |
|                    | `ProductionYear`        | `YEAR`                | Not Null                                                                      |
|                    | `AverageRatingScore`    | `FLOAT`               | Default: 0.0, Check: `0 <= AverageRatingScore <= 5`                           |
|                    | `Director`              | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `Actors`                | `TEXT`                | Not Null                                                                      |
|                    | `Producers`             | `TEXT`                | Not Null                                                                      |
|                    | `Comments`              | `TEXT`                | Nullable                                                                      |
| **Company**        | `CompanyID`             | `INT`                 | Primary Key, Not Null                                                         |
|                    | `Name`                  | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `YearOfEstablishment`   | `YEAR`                | Not Null                                                                      |
|                    | `PhoneNumber`           | `VARCHAR(20)`         | Not Null                                                                      |
|                    | `Location`              | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `Email`                 | `VARCHAR(255)`        | Unique, Not Null                                                              |
| **Comment**        | `CommentID`             | `INT`                 | Primary Key, Not Null                                                         |
|                    | `UserID`                | `INT`                 | Foreign Key (User), Not Null                                                  |
|                    | `MediaID`               | `INT`                 | Foreign Key (Media), Not Null                                                 |
|                    | `Text`                  | `TEXT`                | Not Null                                                                      |
| **Episode**        | `EpisodeID`             | `INT`                 | Primary Key, Not Null                                                         |
|                    | `SeriesID`              | `INT`                 | Foreign Key (Series), Not Null                                                |
|                    | `Title`                 | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `StorageID`             | `INT`                 | Foreign Key (StorageLocation), Not Null                                       |
| **Movie**          | `MovieID`               | `INT`                 | Primary Key, Not Null                                                         |
|                    | `StorageID`             | `INT`                 | Foreign Key (StorageLocation), Not Null                                       |
|                    | `MediaID`               | `INT`                 | Foreign Key (Media), Not Null                                                 |
|                    | `Title`                 | `VARCHAR(255)`        | Not Null                                                                      |
| **Series**         | `SeriesID`              | `INT`                 | Primary Key, Not Null                                                         |
|                    | `MediaID`               | `INT`                 | Foreign Key (Media), Unique, Not Null                                         |
|                    | `Title`                 | `VARCHAR(255)`        | Not Null                                                                      |
| **StorageLocation**| `StorageID`             | `INT`                 | Primary Key, Not Null                                                         |
|                    | `Server`                | `VARCHAR(255)`        | Not Null                                                                      |
|                    | `Path`                  | `VARCHAR(500)`        | Not Null                                                                      |
| **Rating**         | `RatingID`              | `INT`                 | Primary Key, Not Null                                                         |
|                    | `UserID`                | `INT`                 | Foreign Key (User), Not Null                                                  |
|                    | `MediaID`               | `INT`                 | Foreign Key (Media), Not Null                                                 |
|                    | `Score`                 | `INT`                 | Not Null, Check: `1 <= Score <= 5`                                            |
| **WatchLater**     | `ListID`                | `INT`                 | Primary Key, Not Null                                                         |
|                    | `UserID`                | `INT`                 | Foreign Key (User), Not Null                                                  |
|                    | `MediaID`               | `INT`                 | Foreign Key (Media), Not Null                                                 |

