# Design and Implementation of a Database for a Media Streaming Service - Phase 1

## Overview

In this project, I designed and built a database for a **Media Streaming Service**. The database stores critical information such as:

* User profiles
* Subscription details
* Payment histories
* Personalized watch lists

Additionally, it manages media assets, including movies and series with multiple episodes while tracking their storage locations and associated production companies. The system ensures that all data is efficiently organized to support user activities like:

* Leaving comments
* Rating media
* Curating a watch list for future viewing

## Specifications

The database was designed with the following features:

* Each user can subscribe to the service for a specific period. The subscription start and end dates (including subscription tier) are stored.
* The users' payment history, including related subscription and transaction details, is tracked.
* A media entity can be a movie or a series and includes:

  * Genre
  * Production year
  * Average rating score
  * Director
  * Actors
  * Producers
  * List of comments left by users
* Users can leave multiple comments for a media entity but can only rate each item once.
* A series consists of multiple episodes. Each movie or episode of a series must have a storage location, which stores the server and path of the media file.
* Each movie or series belongs to a production company, and its details (name, year of establishment, and contact information) are stored.
* Each user has a ‘Watch Later’ list to which specific movies or episodes of a series can be added.

## Requirements Analysis

### Entities

1. **User**: Represents users of the streaming service, storing their personal and subscription information.

   * `UserID`, `Username`, `Password`, `Email`, `SubscriptionTier`, `SubscriptionStartDate`, `SubscriptionEndDate`
2. **Payment**: Records payment details for users' subscriptions.

   * `PaymentID`, `UserID`, `AmountPaid`, `SubscriptionID`
3. **Media**: Stores information about movies and series.

   * `MediaID`, `Title`, `Type`, `Genre`, `ProductionYear`, `AverageRatingScore`, `Director`, `Actors`, `Producers`, `Comments`
4. **Company**: Represents production companies.

   * `CompanyID`, `Name`, `YearOfEstablishment`, `PhoneNumber`, `Location`, `Email`
5. **Comment**: Stores users' comments on media.

   * `CommentID`, `UserID`, `MediaID`, `Text`
6. **Episode**: Represents individual episodes in a series.

   * `EpisodeID`, `SeriesID`, `Title`, `StorageID`
7. **Movie**: Represents movies in the system.

   * `MovieID`, `StorageID`, `MediaID`, `Title`
8. **Series**: Represents series, which consist of multiple episodes.

   * `SeriesID`, `MediaID`, `Title`
9. **StorageLocation**: Stores details about the storage of media files.

   * `StorageID`, `Server`, `Path`
10. **Rating**: Stores ratings for media items given by users.

    * `RatingID`, `UserID`, `MediaID`, `Score`
11. **WatchLater**: Tracks the "Watch Later" list for users.

    * `ListID`, `UserID`, `MediaID`

### Relationships

The relationships between entities are as follows:

| Entity1       | Entity2         | Type         | Explanation                                                       | Constraints                                                                     |
| ------------- | --------------- | ------------ | ----------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| User          | Payment         | One-to-Many  | A user can make multiple payments.                                | `UserID` in **Payment** references `UserID` in **User**.                        |
| User          | Comment         | One-to-Many  | A user can leave multiple comments on media items.                | `UserID` in **Comment** references `UserID` in **User**.                        |
| User          | Rating          | One-to-Many  | A user can rate multiple media items, but only once per item.     | Composite unique key `(UserID, MediaID)` in **Rating**.                         |
| User          | Watch Later     | Many-to-Many | A user can save multiple media items to their "Watch Later" list. | `UserID` and `MediaID` as a composite key in **WatchLater**.                    |
| Media         | Company         | Many-to-One  | Many media items can belong to a single production company.       | `CompanyID` in **Media** references `CompanyID` in **Company**.                 |
| Series        | Episode         | One-to-Many  | A series can have multiple episodes.                              | `SeriesID` in **Episode** references `SeriesID` in **Series**.                  |
| Movie/Episode | StorageLocation | One-to-One   | Each movie or episode has one unique storage location.            | `StorageID` in **Movie/Episode** references `StorageID` in **StorageLocation**. |
| Media         | Comment         | One-to-Many  | Each media item can have multiple comments.                       | `MediaID` in **Comment** references `MediaID` in **Media**.                     |
| Media         | Rating          | One-to-Many  | Each media item can have multiple ratings.                        | `MediaID` in **Rating** references `MediaID` in **Media**.                      |

## Conceptual Database Design

### Enhanced Entity-Relationship (EER) Diagram

An **EER diagram** represents entities, attributes, and relationships visually. The design incorporates entities such as **User**, **Payment**, **Media**, **Comment**, **Episode**, **Rating**, and **WatchLater**.

### UML Diagram

The **UML diagram** provides a structural view of the system, showcasing the relationship between objects and data flow. This is useful for understanding the interaction between various components, such as user profiles, subscription details, and media entities.

### Step-by-Step Explanation

Each entity and its relationships are carefully selected to reflect the key functionalities of a media streaming service:

* **Users** are linked to their **Payments**, **Comments**, **Ratings**, and **Watch Later** lists, supporting interactions like leaving feedback and managing media preferences.
* **Media** items (movies and series) are linked to **Production Companies** and have a direct relationship with **Comments** and **Ratings** for user feedback.

### Constraints

All tables are designed with various constraints to maintain data integrity:

* **Primary Keys**: Uniquely identify each record in the table.
* **Foreign Keys**: Ensure referential integrity by linking related data across tables (e.g., linking **Payment** to **User**).
* **Unique Constraints**: Ensure that certain data (e.g., **Username**, **Email**) is unique across records.

## Submission Instructions

The final deliverables include:

* A **complete** report summarizing the project, diagrams, and explanations.
* **EER** and **UML** diagrams with clear labeling and structure.
* A **step-by-step breakdown** of each entity and its relationships, including rationale for design choices.
* A detailed description of **constraints**, including primary keys, foreign keys, unique constraints, and data types.
