Using a API
===========

Most of the time, when you want to use an API and access personal account informations, you will use `OAuth2 <https://oauth.net/2/>`_ authentication so you will need to generate a client ID and client secret keys.

.. warning::
    It's important to save the keys in a secure environment ! (Don't write the keys directly into your script..!)
    
In my case I'm generally not playing with sensitive data so I'm doing the minimum: I save the keys as environment variables in my `.profile`:

.. code-block:: bash

    # Spotify keys
    export SPOTIFY_API_KEY=my_spotify_api_key
    export SPOTIFY_SECRET_KEY=my_spotify_secret_key


And then, retrieve the these data in my script:

.. code-block:: python

    import os
    client_id = os.environ["SPOTIFY_API_KEY"]
    client_secret = os.environ["SPOTIFY_SECRET_KEY"]

At this step, you will need to send the keys to the authentication URL to get an access token to include in the requests to authenticate.

.. important::
    Most of the time, you can find a web API wrapper library. If you don't find one quoted (better because ~official) in the official documentation, then you can also search for one in github.


In python, the `requests <https://pypi.org/project/requests/>`_ package is often used.

.. code-block:: python

    conda install -c anaconda requests 

Below is an example for the Spotify API.

Examples
########

Spotify API
***********

- API documentation: https://developer.spotify.com/documentation/web-api/quick-start/
- Generating a client id and client secret key: https://developer.spotify.com/dashboard/login
- API URL: https://api.spotify.com/v1
- OAUTH2 URL: https://accounts.spotify.com/api/token


First example using the `Client Credentials Flow <https://developer.spotify.com/documentation/general/guides/authorization/client-credentials/>`_.
With this method some end points cannot be accessed because it requires extra `scopes <https://developer.spotify.com/documentation/general/guides/authorization/scopes/>`_.

.. code-block:: python

    import requests
    import os
    import base64

    # Urls
    API_URL = "https://api.spotify.com/v1"
    OAUTH_URL = "https://accounts.spotify.com/api/token"

    # Get the client ID and secret
    client_id = os.environ["SPOTIFY_API_KEY"]
    client_secret = os.environ["SPOTIFY_SECRET_KEY"]

    # Open a session
    s = requests.Session()

    # Get the access token
    headers = {
        "Authorization": "Basic " + base64.urlsafe_b64encode(f"{client_id}:{client_secret}".encode()).decode()
    }
    response = s.post(
        url=OAUTH_URL, 
        data={"grant_type": "client_credentials"},
        headers=headers,
        verify=True,
    )
    response.raise_for_status()
    access_token = response.json()["access_token"]

    # Example of a request
    response = s.get(
        url = API_URL + "/artists/0TnOYISbd1XYRBk9myaseg/albums",
        headers = {"Authorization": f"Bearer {access_token}"},
    )
    response.raise_for_status()
    artist_albums = response.json()


Second example using the `Authorization Code Flow <https://developer.spotify.com/documentation/general/guides/authorization/code-flow/>`_.
With this authentication method we can ask access of extra `scopes <https://developer.spotify.com/documentation/general/guides/authorization/scopes/>`_

.. code-block:: python

    import requests
    import os
    import base64
    from urllib.parse import urlencode

    # Urls
    API_URL = "https://api.spotify.com/v1"
    OAUTH_URL = "https://accounts.spotify.com/api/token"

    # Get the client ID and secret
    client_id = os.environ["SPOTIFY_API_KEY"]
    client_secret = os.environ["SPOTIFY_SECRET_KEY"]


    # Build the URL to get the access to the scopes desired
    # Same redirect url need to be set in your spotify app
    redirect_uri = "http://localhost:8888/callback"
    payload = {
        "client_id": client_id,
        "response_type": "code",
        "redirect_uri": redirect_uri,
        "scope": "user-read-private user-read-email user-library-read",
    }
    url_authorize = f"https://accounts.spotify.com/authorize?{urlencode(payload)}"

    # 0. Open "url_authorize" into your browser, accept the access requests:
    #    Example: https://accounts.spotify.com/authorize?client_id=96326hzefijbapocn982374628642&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%3A8888%2Fcallback&scope=user-read-private+user-read-email+user-library-read
    #    Returned URL: http://localhost:8888/callback?code=long_code_to_extract
    # 1. Copy the code in the redirected URL (It's normal if your browser give a "Unable to connect error")
    #    code = long_code_to_extract
    code = "long_code_to_extract"

    # Open a session
    s = requests.Session()

    # Get the access token 
    headers = {
        "Authorization": "Basic " + base64.urlsafe_b64encode(f"{client_id}:{client_secret}".encode()).decode()
    }
    response = s.post(
        url="https://accounts.spotify.com/api/token", 
        data={
            "grant_type": "authorization_code",
            "code": code,
            "redirect_uri": redirect_uri
        },
        headers=headers,
        verify=True,
    )
    response.raise_for_status()
    access_token = response.json()["access_token"]

    # You now have access to the end points available with the scopes you
    # specified before: user-read-private user-read-email user-library-read
    response = s.get(
        url = API_URL + "/me/tracks",
        headers = {"Authorization": f"Bearer {access_token}"},
    )
    response.raise_for_status()
    saved_user_tracks = response.json()

As said earlier, it's better to use a wrapper when available, for the Spotify web API, it exists a python wrapper called `Spotipy <https://github.com/spotipy-dev/spotipy>`_ so let's not reinvent the wheel:
You can install it using the conda command:

.. code-block:: bash

    conda install -c conda-forge spotipy

.. code-block:: python

    import os
    import spotipy
    from spotipy.oauth2 import SpotifyOAuth

    # Get the client ID and secret
    client_id = os.environ["SPOTIFY_API_KEY"]
    client_secret = os.environ["SPOTIFY_SECRET_KEY"]

    # Same as the one defined in my spotify app
    redirect_uri = "http://localhost:8888/callback"

    sp = spotipy.Spotify(auth_manager=SpotifyOAuth(client_id=client_id,
                                                client_secret=client_secret,
                                                redirect_uri=redirect_uri,
                                                scope="user-library-read"))

    user_saved_tracks = sp.current_user_saved_tracks()



------------------------------------------------------------

**Sources**:

- OAuth2: https://oauth.net/2/
- Spotipy: https://github.com/spotipy-dev/spotipy
