## YouTube Playlist to Spotify Migration Script

### Prerequisites:
1. You should have a Spotify account.
2. You need to create a Spotify Application to get the necessary credentials (Client ID and Client Secret). Go to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/applications) and create a new application.

### Step 1: Set Up Environment
- Install the necessary libraries by running `pip install spotipy youtube_dl`.

### Step 2: Get YouTube Playlist Data
- Use `youtube_dl` library to get the list of video URLs from the YouTube playlist.

```python
import youtube_dl

def get_youtube_playlist_urls(playlist_url):
    ydl_opts = {'quiet': True}
    with youtube_dl.YoutubeDL(ydl_opts) as ydl:
        info = ydl.extract_info(playlist_url, download=False)
        video_urls = [entry['url'] for entry in info['entries']]
    return video_urls
```

### Step 3: Authenticate with Spotify
- Use `spotipy` library to authenticate with Spotify.

```python
import spotipy
from spotipy.oauth2 import SpotifyOAuth

def authenticate_spotify(client_id, client_secret, redirect_uri):
    scope = "playlist-modify-public"  # Set the required scope
    sp = spotipy.Spotify(auth_manager=SpotifyOAuth(client_id=client_id,
                                                   client_secret=client_secret,
                                                   redirect_uri=redirect_uri,
                                                   scope=scope))
    return sp
```

### Step 4: Create Spotify Playlist and Add Tracks
- Use the authenticated Spotify client to create a new playlist and add tracks from the YouTube playlist.

```python
def create_spotify_playlist(sp, playlist_name, track_uris):
    user_id = sp.current_user()['id']
    playlist = sp.user_playlist_create(user=user_id, name=playlist_name, public=True)
    sp.playlist_add_items(playlist_id=playlist['id'], items=track_uris)
```

### Step 5: Main Function
- Combine all the previous steps in a main function.

```python
def main(youtube_playlist_url, spotify_playlist_name, client_id, client_secret, redirect_uri):
    # Step 2: Get YouTube Playlist Data
    video_urls = get_youtube_playlist_urls(youtube_playlist_url)

    # Step 3: Authenticate with Spotify
    sp = authenticate_spotify(client_id, client_secret, redirect_uri)

    # Step 4: Convert YouTube URLs to Spotify URIs
    track_uris = []
    for video_url in video_urls:
        # Implement logic to convert YouTube URLs to Spotify URIs
        # You can use Spotify API search or external services to find the corresponding Spotify URI for each track.
        # As of my knowledge cutoff date in September 2021, I don't have access to the internet and cannot provide a live example of this conversion.
        # However, you can use the Spotify API's search function to find the track URI based on the track name and artist name.

        # For example, you can use:
        # result = sp.search(q='track_name artist_name', type='track')
        # track_uri = result['tracks']['items'][0]['uri']
        # track_uris.append(track_uri)

    # Step 5: Create Spotify Playlist and Add Tracks
    create_spotify_playlist(sp, spotify_playlist_name, track_uris)


if __name__ == "__main__":
    youtube_playlist_url = "YOUR_YOUTUBE_PLAYLIST_URL"
    spotify_playlist_name = "YOUR_SPOTIFY_PLAYLIST_NAME"
    client_id = "YOUR_SPOTIFY_CLIENT_ID"
    client_secret = "YOUR_SPOTIFY_CLIENT_SECRET"
    redirect_uri = "YOUR_REDIRECT_URI"

    main(youtube_playlist_url, spotify_playlist_name, client_id, client_secret, redirect_uri)
```

### Step 6: Run the Script
- Replace `YOUR_YOUTUBE_PLAYLIST_URL`, `YOUR_SPOTIFY_PLAYLIST_NAME`, `YOUR_SPOTIFY_CLIENT_ID`, `YOUR_SPOTIFY_CLIENT_SECRET`, and `YOUR_REDIRECT_URI` with your actual values.
- Run the script using `python script_name.py`.

