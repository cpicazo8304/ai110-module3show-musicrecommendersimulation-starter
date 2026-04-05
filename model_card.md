# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

**AuraTrack 1.0**

## 2. Intended Use  

This recommender is designed to suggest songs from a catalog that best match a user's listening preferences based on audio features and mood. It assumes the user has some general sense of what they like. This is primarily a classroom exploration project, built to demonstrate how a weighted similarity scoring system works in practice. However, the  logic works like the real-world music recommenders used by platforms like Spotify.

## 3. How the Model Works  

This music recommender includes traits such as energy, acousticness, tempo, mood, genre, and more. Each trait is compared against what the listener prefers. Each trait has a numerical value from 0 to 1. Not all traits are weighted equally. Energy and acousticness carry more influence than genre or artist name (because of the size of the data). These traits define how a song feels more than anything else. Like, when you going to the gym, you want a song with more energy. If you are studying, you would want the opposite. Every song is given a score based on how their traits compare with the user's preferences. It is computed with a simple subtraction. Smaller differences are better since that means the numerical values were close. The catalog then gets sorted from highest to lowers, and the top results are the recommendations.

## 4. Data  

- The catalog contains 20 songs, each represented as a row in songs.csv with 10 features: id, title, artist, genre, mood, energy, tempo_bpm, valence, danceability, and acousticness.
- Genres represented (17 total): ambient, classical, country, EDM, electronic, folk, gospel, hip-hop, indie pop, jazz, lofi, metal, pop, reggae, rock, synthwave, and world. Coverage is broad but shallow — most genres have only one song representing them, meaning a single song carries the entire weight of its genre in recommendations.

- Moods represented (13 out of 16): chill, energetic, exhilarated, focused, happy, hopeful, inspired, intense, laid-back, melancholic, moody, nostalgic, peaceful, playful, relaxed, and tribal are all defined in the similarity matrix, but the catalog only uses a subset of them. 

- No data was added or removed — the original 20-song catalog was used as-is throughout all testing.

- Missing aspects: 
    - Scale: 20 songs is far too small for a real recommender. Many profiles returned the same songs regardless of weight changes, simply because there aren't enough candidates.
    - Sub-genre nuance: there's no distinction between, say, hard rock and indie rock, or deep house and tech EDM.
    -Lyrics: could add to the mood.

## 5. Strengths  

The system works best for users with clear, consistent preferences, profiles like High-Energy Pop and Deep Intense Rock consistently produced intuitive top results like Storm Runner, Winter Solitude, and Code in Motion across multiple weight configurations. The fuzzy similarity matrices are a genuine strength, allowing the mood and genre components to reward near-matches rather than punishing any deviation from an exact label, which meant chill-adjacent moods like "focused" and "peaceful" still surfaced correctly for the Chill Lofi profile. The per-feature explanation output also proved valuable during testing, making it immediately clear why each song ranked where it did and allowing weight changes and bug fixes to be verified at a glance.


## 6. Limitations and Bias 

- One limitation is that for my similarity matrix, it only works with the current categorical values.
If a new category is added, it may not view that category as anything important which could ruin results.
So, it would need to update every time there is something new, or need to add a large amount of categorical values beforehand.

- Another is dataset limitation. There are multiple edge cases that cannot be tested with limited number of datasets. My system requires more data to be properly tested and reviewed. 

- Also, the system is biased towards to the higher weights. It becomes a bit too deterministic. Maybe it would be better to make it stochastic/random by ranking them with a probability distribution. 

## 7. Evaluation  

I tested the High Energy Pop, Chill Lofi, and Deep Intense Rock user profiles. I looked for if the genres of songs matched the user profiles or were related. I also checked for the energy and mood, which usually tells you if a song is for situations where you want energy like the gym (High Energy Pop) or not like when studying (chill lofi). 

They were able to behave like they were supposed to be. Each of the user profiles had a top 5 of songs that made sense in terms how much energy they had, the mood they had, and if the genre made sense. Like for Chill Lofi, it made sense to have more calming music. Most were lofi songs but some were other types of genres like folk or classical, which made sense since these type of genres are more relaxed with less energy. There wasn't anything suprising.

I also tested 3 edge case user profiles: Dead Center, Impossible Unicorn, Conflicting Energy/Mood. 
- Dead Center had the middle numerical value for all the features. This would make it difficult to choose which songs for the top recommendations, it should be mixed and small changes to tie breakers would change the order easily. 

- Impossible Unicorn's had preferences that no single song in the CSV satisfies. For example, high energy AND high acousticness AND slow tempo. Should still rank something on top but the scores should be low. The top was 0.63 which was low compared to other user profiles. 

- Conflicting energy/mood tested a user profile that wanted high energy but wants melancholic/peaceful mood. Scores will be pulled in opposite directions. I wanted to see which would win out. Energy had the bigger weight so made sense that it was the deciding factor in the rankings.


## 8. Future Work  

A more adaptive similarity matrix/weighing system would make sense. Like if extra categorical values are included, it would be nice to have a matrix that would adjust. Also, if more features are included, weighting can be adjusted accordingly. Also, having a probability distribution over the scores of each song, and having a sampling process that gets the top 5 recommendations (leaves the door open for users to explore songs that may not be in their kind of music). 

## 9. Personal Reflection  

There is a lot that you have to think about with recommendation systems. It is not as simple as removing and adding points. You need to think about the scoring system that can best predict what a user would like. Also, there could be users that don't mind exploring new songs, where you have to leave the door open to introducing them to new music other than the same songs. I believe this is where collaborative filtering comes in. 
