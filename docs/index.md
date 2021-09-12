## Getting Started

### Prerequisites:

- [Neo4j Desktop](https://neo4j.com/download/) with database version 3.5.25
- [MongoDB Community Edition](https://docs.mongodb.com/manual/tutorial/)
- [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/), version 1.8.
- [Apache Maven](https://maven.apache.org/), version 3.0 or later.
- [Java Extension Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
- [Spring Boot Extension Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Pivotal.vscode-boot-dev-pack)

After installing the prerequisites, load the project and Maven and Spring Boot tools should show automagically.

<img src="https://raw.githubusercontent.com/navn-r/spotify-api-clone/main/docs/springboot.png" height="200">   

Press `Start` in the Spring Boot Dashboard and select both microservices

**Note:** Both the Neo4j and MongoDB databases should be running **before** starting both microservices.  

By default, the Song Microservice runs on port `3001`, and port `3002` for the Profile Microservice.

#### For MongoDB

- Database Name: `“csc301-test”`
- Collection Name: `"songs"`
- Connected to port `27107` 

#### For Neo4j

- Database username and password: `"neo4j"` and `"1234"` respectively
- Connected to port `7687`

<br />

---

<br />

## Song Microservice    

### <span class="label label-success">GET</span>  `/getSongTitleById/{songId}`
> Retrieves the song title given the `songId`.

**URL Parameters**  
  - `songId` is the _id of a specific song  
  
**Expected Response**  
  - `OK`, if the title was retrieved successfully  
  - `<string>`, any other status if the song was not retrieved successfully  

**Example Response**
```JSON
{
  "status": "OK",
  "data": "Never gonna give you up"
}
```

<br />
<br />



### <span class="label label-danger">DELETE</span>  `/deleteSongById/{songId}`
> Deletes the song from MongoDB and all Profiles that have added it to their favorites list.

**URL Parameters**
  - `songId` is the _id of a specific song  
  
**Expected Response**  
  - `OK`, if the song was deleted successfully  
  - `<string>`, any other status if the song was not deleted successfully  

<br />
<br />




### <span class="label label-warning">POST</span>  `/addSong`
> Adds a song to the database.

**Query Parameters**  
  - `songName` : the name of the song
  - `songArtistFullName` : the name of the song artist
  - `songAlbum` : the album of the song  
  
**Expected Response**  
  - `status`
    - `OK`, if the song was deleted successfully
    - `<string>`, any other status if the song was not deleted successfully
  - `data`
    - song object

**Example Response**
```JSON
{
  ​"data"​: {
    ​"songName"​: ​"testDeleteSong"​,
    ​"songArtistFullName"​: ​"111"​,​
    "songAlbum"​: ​"testDeleteSongArtist"​,
    ​"songAmountFavourites"​: ​"0"​,
    ​"id"​: ​"5d65df689a2efe318808f4cc"    
  },
  ​"status"​: ​"OK"
}
```


<br />
<br />



### <span class="label label-primary">PUT</span> `/updateSongFavouritesCount/{songId}?shouldDecrement=`
> Updates the song's favorites count.

**URL Parameters**
  -  `songId` is the _id of a specific song
  
**Query Parameters**
  - `shouldDecrement` : string values of `true` or `false`
    - states where the song count should be incremented or decremented by 1

**Expected Response**
  - `status`
    - `OK`, if the song was updated successfully
    - `<string>`, any other status if the song was not updated successfully

<br />

---

<br />

## Profile Microservice  

### <span class="label label-warning">POST</span> `/profile`
> Adds a profile to the database and creates a liked songs playlist.

**Query Parameters**
  - `userName` : name of the profile
  - `fullName` : full name of the User
  - `password` : password of the profile
  
**Expected Response**
  - `status`
    - `OK`, if the profile was created successfully
    - `<string>`, any other status if the profile was not created successfully


<br />
<br />



### <span class="label label-primary">PUT</span>  `/followFriend/{username}/{friendUserName}`
> Allows a profile to follow another profile.

**URL Parameters**
  - `userName`  : the username of the profile that will follow `friendUserName`
  - `friendUserName` : the username of the profile that will be followed
  
**Expected Response**
  - `status`
    - `OK`, if the the user was able to follow successfully
    - `<string>`, any other status if the the user was not able to follow successfully


<br />
<br />



### <span class="label label-primary">PUT</span>  `/unfollowFriend/{username}/{friendUserName}`
> Allows a profile to unfollow another profile.

**URL Parameters**
  - `userName`  : the username of the profile that will unfollow `friendUserName`
  - `friendUserName` : the username of the profile that will be unfollowed

**Expected Response**
  - `status`
    - `OK`, if the the user was able to unfollow successfully
    - `<string>`, any other status if the the user was not able to unfollow successfully


<br />
<br />




### <span class="label label-primary">PUT</span>  `/likeSong/{userName}/{songId}`
> Allows a profile to like a song and add it to their liked songs playlist.

**URL Parameters**
  - `userName`  : the username of the profile that will like the song
  - `songId` is the _id of a specific song

**Expected Response**
  - `status`
    - `OK`, if the the user was able to like the song successfully
    - `<string>`, any other status if the the user was not able to like the song successfully


<br />
<br />




### <span class="label label-primary">PUT</span>  `/unlikeSong/{userName}/{songId}`
> Allows a profile to unlike a song and add it to their liked songs playlist.

**URL Parameters**
  - `userName`  : the username of the profile that will unlike the song
  - `songId` is the _id of a specific song

**Expected Response**
  - `status`
    - `OK`, if the the user was able to unlike the song successfully
    - `<string>`, any other status if the the user was not able to unlike the song successfully


<br />
<br />




### <span class="label label-success">GET</span> `/getAllFriendFavouriteSongTitles/{userName}`
> Returns all the song names of the songs that the user's friends have liked.

**URL Parameters**
  - `userName` : the username of the profile whose friends songs we will retrieve
  - `songId` is the _id of a specific song

**Expected Response**
  - `status`
    - `OK`, if the names of songs were retrieved successfully
    - `<string>`, any other status if the names of songs were not retrieved successfully
  - `data`
    - list of songs

**Example Response**
```JSON
{​
  "data"​: {​
    "tahmid"​: [
        ​"The Lego Movie"​,
        ​"Henry IV"​,​
        "Land of Milk and Honey"
    ],​
    "shabaz"​: [
        ​"Sliver"​,
        ​"Off the Black"​,​
        "The Lego Movie"
    ]    
  },​
  "status"​: ​"OK"
}
```
