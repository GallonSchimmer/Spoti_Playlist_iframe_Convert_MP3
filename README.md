# Spoti_Playlist_iframe_Convert_MP3


This repository contains a web application that enables users to fetch the first 50 tracks from a Spotify playlist and displays links to the same tracks on Apple Music and YouTube. Additionally, it provides an iframe to show YouTube search results and another for downloading videos, intended strictly for educational use.

## Disclaimer

This project is designed for academic and learning purposes only. It demonstrates API integration and client-side JavaScript functionality. Please ensure that all use of this tool complies with Spotify's, YouTube's, and Apple Music's terms of service.

## Features

- Fetch and display the first 50 tracks from a specified Spotify playlist.
- Display corresponding Apple Music and YouTube links for each track.
- Embed YouTube search results and a generic downloader iframe (for educational use only).

## Setup

No additional setup is required for static pages. However, ensure you have a modern browser that supports ES6+ features for JavaScript execution.

## Usage

To use the application, follow these steps:

1. Open `index.html` in your browser.
2. Enter the Spotify playlist link in the input field provided.
3. Click on "Get Playlist Tracks" to load the tracks and their corresponding links.

Ensure you replace `clientId` and `clientSecret` in the JavaScript with your Spotify API credentials.

## Example Code

Below is a snippet from the JavaScript used to fetch playlist tracks from Spotify:

```javascript
async function getPlaylistTracks() {
    const clientId = 'your-client-id'; // Replace with your Spotify client ID
    const clientSecret = 'your-client-secret'; // Replace with your Spotify client secret
    const playlistLink = document.getElementById('playlistLink').value;
    const playlistId = playlistLink.split('playlist/')[1].split('?')[0];

    // Fetch Spotify access token
    const responseToken = await fetch('https://accounts.spotify.com/api/token', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}`
    });
    const dataToken = await responseToken.json();
    const accessToken = dataToken.access_token;

    // Fetch tracks using the access token
    const headers = {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json',
    };
    const responseTracks = await fetch(`https://api.spotify.com/v1/playlists/${playlistId}/tracks?limit=50`, { headers });
    const dataTracks = await responseTracks.json();

    // Process and display tracks
    const table = document.getElementById('tracksTable');
    dataTracks.items.forEach(item => {
        const track = item.track;
        const name = track.name;
        const artists = track.artists.map(artist => artist.name).join(', ');
        const row = document.createElement('tr');
        row.innerHTML = `<td>${name}</td><td>${artists}</td><td><a href="${getMediaLink(name, artists, 'iTunes')}" target="_blank">Apple Music</a></td><td><a href="#" onclick="showYouTubeResults('${getMediaLink(name, artists, 'YouTube')}')">YouTube</a></td>`;
        table.appendChild(row);
    });
}
```


## License

This project is open-source and available under the MIT License.
```

### Notes
- Replace placeholders such as `your-username`, `your-client-id`, and `your-client-secret` with actual values.
- The example code snippet demonstrates a basic API call to Spotify and the dynamic generation of HTML content based on fetched data.
- Modify the contributing and license sections as necessary for your project's governance and sharing policies.
