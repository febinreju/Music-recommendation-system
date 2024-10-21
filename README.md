# 🎶 Music Recommender System

This repository contains a **Music Recommender System** that suggests songs based on the user's input. The system leverages **Spotify API** for fetching album covers and artist information, **Natural Language Processing (NLP)** techniques for recommendation, and a pre-trained similarity model. The web interface is built using **Streamlit** for easy user interaction.

## 🎯 Features

- **Song Recommendation**: Input a song, and the system recommends five similar songs.
- **Album Cover Display**: For each recommended song, the corresponding album cover is fetched from the **Spotify API**.
- **Interactive Web App**: The user interface is built with **Streamlit**, providing an easy-to-use, interactive experience.
- **Machine Learning Model**: Utilizes song similarity matrix for generating recommendations.

## 📂 Dataset

The dataset used for this project is the **Spotify Million Song Dataset**, which contains extensive metadata on millions of songs. You can download the dataset from **Kaggle**:

[Kaggle Dataset: Spotify Million Song Dataset](https://www.kaggle.com/datasets/notshrirang/spotify-million-song-dataset)

### Dataset Description

- **Columns**:  
  - `song`: The title of the song.
  - `artist`: The name of the artist who performed the song.
  - Other metadata that may include song duration, genre, etc.

### Model Information

- The similarity between songs is computed based on pre-processed features from the dataset.
- Similarity is stored as a matrix, which is loaded from a pre-trained file using `pickle`.

## 🛠️ Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/febinreju/music-recommender-system.git
    cd music-recommender-system
    ```

2. **Install the required dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

    Dependencies include:
    - Streamlit
    - Spotipy
    - Pandas
    - Pickle

3. **Set up Spotify API**:
    - Create a Spotify Developer account at [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/).
    - Create a new application and get your **Client ID** and **Client Secret**.
    - Replace the placeholders in the code with your **CLIENT_ID** and **CLIENT_SECRET**.

4. **Download the Kaggle dataset**:
    - Download the dataset from the following link: [Spotify Million Song Dataset](https://www.kaggle.com/datasets/notshrirang/spotify-million-song-dataset).
    - Store the dataset in the project directory and preprocess it as needed (details provided below).

5. **Run the app**:
    ```bash
    streamlit run app.py
    ```

## 📊 How it works

1. **Input a Song**: The user selects a song from the dropdown menu (populated from the Kaggle dataset).
2. **Recommendation**: Based on the selected song, the app recommends five similar songs using a pre-trained similarity matrix.
3. **Album Cover**: The album covers for the recommended songs are fetched from the Spotify API and displayed alongside the song names.

## 🧠 Code Walkthrough

### 1. Loading Data

- **Pickle** is used to load the pre-processed data (songs and similarity matrix) into the application.
- The Kaggle dataset is preprocessed and saved into two files:
  - `df.pkl` – the song and artist data.
  - `similarity.pkl` – the similarity matrix for song recommendations.

### 2. Spotify API Integration

The **Spotify API** is used to retrieve album cover images for the recommended songs. A fallback image is provided if no results are found:

```python
def get_song_album_cover_url(song_name, artist_name):
    search_query = f"track:{song_name} artist:{artist_name}"
    results = sp.search(q=search_query, type="track")

    if results and results["tracks"]["items"]:
        track = results["tracks"]["items"][0]
        album_cover_url = track["album"]["images"][0]["url"]
        return album_cover_url
    else:
        return "https://i.postimg.cc/0QNxYz4V/social.png"  # Fallback image URL
