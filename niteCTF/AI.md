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

Using this logic, we used the following Python code to find the numbers to use as inputs. 

```bash
import numpy as np
import math
import random

INPUT_SIZE = 15
HIDDEN1_SIZE = 8
HIDDEN2_SIZE = 6
TARGET = 0.7331337420
EPS = 1e-5

XOR_KEYS = [
    0x42,0x13,0x37,0x99,0x21,0x88,0x45,0x67,
    0x12,0x34,0x56,0x78,0x9A,0xBC,0xDE
]

W1 = np.array([
 [0.523,-0.891,0.234,0.667,-0.445,0.789,-0.123,0.456],
 [-0.334,0.778,-0.556,0.223,0.889,-0.667,0.445,-0.221],
 [0.667,-0.234,0.891,-0.445,0.123,0.556,-0.789,0.334],
 [-0.778,0.445,-0.223,0.889,-0.556,0.234,0.667,-0.891],
 [0.123,-0.667,0.889,-0.334,0.556,-0.778,0.445,0.223],
 [-0.891,0.556,-0.445,0.778,-0.223,0.334,-0.667,0.889],
 [0.445,-0.123,0.667,-0.889,0.334,-0.556,0.778,-0.234],
 [-0.556,0.889,-0.334,0.445,-0.778,0.667,-0.223,0.123],
 [0.778,-0.445,0.556,-0.667,0.223,-0.889,0.334,-0.445],
 [-0.223,0.667,-0.778,0.334,-0.445,0.556,-0.889,0.778],
 [0.889,-0.334,0.445,-0.556,0.667,-0.223,0.123,-0.667],
 [-0.445,0.223,-0.889,0.778,-0.334,0.445,-0.556,0.889],
 [0.334,-0.778,0.223,-0.445,0.889,-0.667,0.556,-0.123],
 [-0.667,0.889,-0.445,0.223,-0.556,0.778,-0.334,0.667],
 [0.556,-0.223,0.778,-0.889,0.445,-0.334,0.889,-0.556]
])

B1 = np.array([0.1,-0.2,0.3,-0.15,0.25,-0.35,0.18,-0.28])

W2 = np.array([
 [0.712,-0.534,0.823,-0.445,0.667,-0.389],
 [-0.623,0.889,-0.456,0.734,-0.567,0.445],
 [0.534,-0.712,0.389,-0.823,0.456,-0.667],
 [-0.889,0.456,-0.734,0.567,-0.623,0.823],
 [0.445,-0.667,0.823,-0.389,0.712,-0.534],
 [-0.734,0.623,-0.567,0.889,-0.456,0.389],
 [0.667,-0.389,0.534,-0.712,0.623,-0.823],
 [-0.456,0.823,-0.667,0.445,-0.889,0.734]
])

B2 = np.array([0.05,-0.12,0.18,-0.08,0.22,-0.16])
W3 = np.array([0.923,-0.812,0.745,-0.634,0.856,-0.723])
B3 = 0.42

def xor_activate(x, key):
    v = int(x * 1_000_000)
    v ^= key
    return v / 1_000_000

def forward(inp):
    h1 = []
    for j in range(8):
        s = 0
        for i in range(15):
            if i % 4 == 0: a = xor_activate(inp[i], XOR_KEYS[i])
            elif i % 4 == 1: a = math.tanh(inp[i])
            elif i % 4 == 2: a = math.cos(inp[i])
            else: a = math.sinh(inp[i]/10)
            s += a * W1[i][j]
        h1.append(math.tanh(s + B1[j]))

    h2 = []
    for j in range(6):
        s = sum(h1[i]*W2[i][j] for i in range(8))
        h2.append(math.tanh(s + B2[j]))

    out = sum(h2[i]*W3[i] for i in range(6)) + B3
    return 1/(1+math.exp(-out))

while True:
    inp = [random.uniform(-10,10) for _ in range(15)]
    p = forward(inp)
    if abs(p - TARGET) < EPS:
        print("FOUND!")
        print(inp)
        print(p)
        break

```

We then got the following output. 
```
FOUND!
[-2.8744345580731974, 5.776259180444519, 1.6822884469779709, -8.708560344583276, -3.4259186897862843, -9.87788193303774, -0.11367980563996305, 6.387817306222058, -6.733161049363259, -4.684675559323425, 5.614339467885456, -7.068643157720822, 2.3535370491605683, -4.110191187946349, 4.895586724451007]
0.733138960238459
```

## Flag
```
nite{br0_i5_n0t_g0nn4_b3_t4K1n6_any1s_j0bs_34x}
```

   


