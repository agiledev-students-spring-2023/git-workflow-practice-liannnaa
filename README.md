## Extracting Song Data From the Spotify API Using Python
![Spotify](https://miro.medium.com/v2/resize:fit:1400/0*B260_-rdgu3tjKxK)
[Extracting Song Data From the Spotify API Using Python](https://towardsdatascience.com/extracting-song-data-from-the-spotify-api-using-python-b1e79388d50)

Song data can be accessed through Spotify's API using Python's Spotipy package.
```python
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
```
### What can be extracted?
* "Feel"
* "Liveness"
* "Acousticness"
* "Energy"
* Song Popularity
* Artist Popularity
* Predicted Position of Each Beat
* More!

## API Keys
1. Create a Spotify for Developers account (same as Spotify account)
2. Go to dashboard and "**create an app**"
3. Access public and private key
4. Get client ID and client secret for app (keep in separate Gitignored file)

## Authenticating with Spotipy
### Authenticate without a specific user in mind
* All you need are the IDs (client and secret)
* Access general features of Spotify and see playlists
* Needed to see specific user stats
    - Following lists
    - Music listened to
```python
# Authentication - without user
client_credentials_manager = SpotifyClientCredentials(client_id=cid, client_secret=secret)
sp = spotipy.Spotify(client_credentials_manager = client_credentials_manager)
```
### Authenticate with an account
* **CANNOT** use methods relating to the current user
* Prompt user to sign in
* Use `prompt_for_user_token` method in the `spotipy.utils` section of the [package](https://spotipy.readthedocs.io/en/2.19.0/)

## Uniform Resource Identifiers
* Point at each object in the API
* Performs any function with the API referring to an object in Spotify
* Contained in its shareable link
###### FOR EXAMPLE (Link to Global Top Songs Playlist)
[https://open.spotify.com/playlist/37i9dQZEVXbNG2KDcFcKOF?si=77d8f5cd51cd478d](https://open.spotify.com/playlist/37i9dQZEVXbNG2KDcFcKOF?si=77d8f5cd51cd478d)

The URI is "*37i9dQZEVXbNG2KDcFcKOF*"

* Use URI to refrence playlist
* Could also be in the format "*spotify:object_type:uri*"
## Extracting Tracks From Playlist
**`playlist_tracks` method**: Takes the URI from a playlist and outputs JSON data (Spotipy decodes this)

### Extract URI for each song
```python
playlist_link = "https://open.spotify.com/playlist/37i9dQZEVXbNG2KDcFcKOF?si=1333723a6eff4b7f"
playlist_URI = playlist_link.split("/")[-1].split("?")[0]
track_uris = [x["track"]["uri"] for x in sp.playlist_tracks(playlist_URI)["items"]]
```
### Extract song info
```python
for track in sp.playlist_tracks(playlist_URI)["items"]:
    #URI
    track_uri = track["track"]["uri"]
    
    #Track name
    track_name = track["track"]["name"]
    
    #Main Artist
    artist_uri = track["track"]["artists"][0]["uri"]
    artist_info = sp.artist(artist_uri)
    
    #Name, popularity, genre
    artist_name = track["track"]["artists"][0]["name"]
    artist_pop = artist_info["popularity"]
    artist_genres = artist_info["genres"]
    
    #Album
    album = track["track"]["album"]["name"]
    
    #Popularity of the track
    track_pop = track["track"]["popularity"]
```
### Extract song features
```python
sp.audio_features(track_uri)[0]
```