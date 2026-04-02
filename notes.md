# Phase 1

## step 2

I definitely agree that energy plays a huge part into the musical "vibe". Sometimes you want something more calming and other times you need something more energizing (depending on the time of day). Also, maybe you are the type of person that loves calming music (maybe too much energy is too loud). Also valence and mood makes sense in how a person may be feeling through a set of days (sad music cuz of a break up or loss), happy music (because they achieved something), etc.

Tempo and danceability makes sense but too specific and only really matters when you want something more energizing but energy is already a feature so that kind of covers for these features.

Artist and genre make more sense when there are way more data. If there is more data, then this would make sense. And tbh, I could listen to anything so these may not matter as much, but sometimes I just get the feeling to listen to a single artist (not exactly genre) for the day or a ride to somewhere.                                                 

## Step 3

-score songs based on their features weights. 
-have each feature have a weighting (all weights add up to one)
-each feature has a weight of importance
-rank in this order:
  -energy
  -mood
  -valence
  -tempo
  -danceability
  -genre
  -artist

## Scoring System and Similarities

### Similarity Scores for Features
- **Numerical features (energy, acousticness, valence, tempo, danceability)**: Use `similarity = 1 - |song_value - user_preferred|`. For tempo (in BPM, not 0-1), normalize: `similarity = 1 - |tempo - pref| / (max_tempo - min_tempo)` (e.g., min=60, max=200).
- **Categorical features**:
  - **Genre and Mood**: Use fuzzy similarity matrices (based on relatedness, 0-1 scale).
  - **Artist**: Exact match: `similarity = 1` if same artist, else `0`.

### Weighted Scoring Formula
Overall song score = sum(weight * similarity for each feature). Weights sum to 1, based on ranking:
- Energy: 0.25
- Acousticness: 0.20 (added between energy and mood)
- Mood: 0.15
- Valence: 0.12
- Tempo: 0.10
- Danceability: 0.08
- Genre: 0.06
- Artist: 0.04

### Genre Similarity Matrix
(7x7, unique genres: ambient, indie pop, jazz, lofi, pop, rock, synthwave)

| Genre      | ambient | indie pop | jazz | lofi | pop | rock | synthwave |
|------------|---------|-----------|------|------|-----|------|----------|
| ambient   | 1.0    | 0.4      | 0.5 | 0.8 | 0.3| 0.2 | 0.6     |
| indie pop | 0.4    | 1.0      | 0.6 | 0.5 | 0.9| 0.7 | 0.5     |
| jazz      | 0.5    | 0.6      | 1.0 | 0.6 | 0.5| 0.4 | 0.4     |
| lofi      | 0.8    | 0.5      | 0.6 | 1.0 | 0.4| 0.3 | 0.5     |
| pop       | 0.3    | 0.9      | 0.5 | 0.4 | 1.0| 0.6 | 0.7     |
| rock      | 0.2    | 0.7      | 0.4 | 0.3 | 0.6| 1.0 | 0.5     |
| synthwave | 0.6    | 0.5      | 0.4 | 0.5 | 0.7| 0.5 | 1.0     |

### Mood Similarity Matrix
(6x6, unique moods: chill, focused, happy, intense, moody, relaxed)

| Mood     | chill | focused | happy | intense | moody | relaxed |
|----------|-------|---------|-------|---------|-------|---------|
| chill   | 1.0  | 0.8    | 0.5  | 0.3    | 0.6  | 0.9    |
| focused | 0.8  | 1.0    | 0.4  | 0.5    | 0.5  | 0.7    |
| happy   | 0.5  | 0.4    | 1.0  | 0.6    | 0.3  | 0.6    |
| intense | 0.3  | 0.5    | 0.6    | 1.0    | 0.7  | 0.4    |
| moody   | 0.6  | 0.5    | 0.3    | 0.7    | 1.0  | 0.5    |
| relaxed | 0.9  | 0.7    | 0.6    | 0.4    | 0.5  | 1.0    |

### A/B Testing
A/B testing compares two versions (A and B) of something (e.g., recommendation algorithm) by assigning users randomly and measuring performance (e.g., via metrics like satisfaction). Use to validate scoring changes.
