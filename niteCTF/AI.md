# Antakshari

## Description 

My friend can never remember movie names, absolutely hopeless. This time he only recalls six cast members, nothing else. He's always been dense, but we've been a tight for years, so I guess I'm stuck helping him again. Can you figure out the movie he's trying to remember?

## Solution 
From the description, it is seen that the 6 cast members denote the 6 nodes that are clustered together that we need to find in the neural network, which then have to be arranged in descending order to get the flag. We then used the K Nearest Neighbors Classification to find the cluster of nodes.
K-Nearest Neighbors Classifier first stores the training examples. During prediction, when it encounters a new instance (or test example) to predict, it finds the K number of training instances nearest to this new instance.  Then assigns the most common class among the K-Nearest training instances to this test instance. Using the nearest neighbours code to find 5 other closest indices to 1 that we choose.
Similarly, when we get 6 nodes coming together everytime, when nearest distance is measured, gives us the classified cluster we need. Like actors of a single movie would be found together at every testing. Rough snippet of the logic is given below.
```bash
import numpy as np
from sklearn.cluster import KMeans

lv = np.load("latent_vectors.npy") #latent vectors embedding

norms = np.linalg.norm(lv, axis=1) #Euclidian length of vectors
labels = KMeans(n_clusters=2, random_state=0).fit(norms.reshape(-1, 1)).labels_ #Forming clusters

movie_label = 1 if norms[labels == 1].mean() > norms[labels == 0].mean() else 0
movies = np.where(labels == movie_label)[0]
actors = np.where(labels != movie_label)[0]

M = lv @ lv.T #Similarity matrix (gives number of items matrix)

for movie in sorted(movies):
    sims = M[movie, actors] #similarity between the movie and each actor
    order = np.argsort(-sims) #most similar sorting
    top6 = actors[order[:6]] #top 6 actors
    seq = ",".join(str(x) for x in sorted(top6, reverse=True)) #sort the 6 actors in reverse order
    print(f"Movie {movie}: seq={seq}")
```
This prints `Movie 3: seq=189,177,134,108,37,29`
Submitting it in the url https://antakshari1.chall.nitectf25.live/, we get the flag.

## Flag
```
nite{Diehard_1891771341083729}
```

# floating-point guardian

## Description 

Look at my digital gurdian. I built it using my custom made neural network written in C. Bad move? eh well.
Connection: ncat --ssl floating.chals.nitectf25.live 1337

## Solution 

After reading through the source program given, we realised that the program essentially just took 15 numbers and put them through certain transformations, after which the final number was compared to a predefined fixed number. 
<img width="324" height="50" alt="Screenshot 2025-12-15 at 9 02 33 PM" src="https://github.com/user-attachments/assets/a2d618cf-73eb-4408-b42c-673f4868d537" />

The transformations take place in four parts:

- The first transformations are individual according to the indices of the inputs. 
<img width="547" height="123" alt="Screenshot 2025-12-15 at 9 09 15 PM" src="https://github.com/user-attachments/assets/704ba1cd-cbaf-45b3-8b4f-0b4c928a8606" />
- In this step, 15 numbers are collapsed into 8 numbers 
<img width="388" height="109" alt="Screenshot 2025-12-15 at 9 13 34 PM" src="https://github.com/user-attachments/assets/4585f8fe-e9fd-40b3-bc13-a8bf1f961229" />
- In the next step, 8 numbers are collapsed into 6 
<img width="317" height="84" alt="Screenshot 2025-12-15 at 9 19 04 PM" src="https://github.com/user-attachments/assets/36839417-fe5a-4937-8d02-c460078ea979" />
- The final step collapses 6 numbers into 1, which is then compared to the `TARGET_PROBABILITY`
   <img width="306" height="76" alt="Screenshot 2025-12-15 at 9 21 16 PM" src="https://github.com/user-attachments/assets/c30704ed-31d3-4164-9666-d3c57263fd75" />


