# Fast Anomaly Detection For Streaming Data

### Core Functions
#### 1. InitialiseWorkSpace(dimensions):

Before the construction of each tree. Assume that attributes’ ranges are normalised to [0, 1] at the outset. Let sq be a real number randomly and uniformly generated from the interval [0, 1]. A work range, sq ± 2 · max(sq, 1 − sq), is defined for every dimension q in the feature space D.

#### 2. BuildSingleHSTree(max_arr, min_arr, k, h, dimensions)

Each internal node is formed by randomly selecting a dimension q to form two half-spaces; the split point is the mid-point of the current range of q. The mass variables of each node, r and l, are initialised to zero during the tree construction process.

#### 3. UpdateMass(x, node, ref_window)

Once HS-Trees are constructed, mass profile of normal data must be recorded in the trees before they can be employed for anomaly detection. The process involves traversing every instance in a window through each HS-Tree. UpdateMass function shows the process where instances in the reference window will update mass r; and mass l is updated using instances in the latest window. These two collections of mass values at each node, r and l, represent the data profiles in the two different windows.

#### 4. ScoreTree(x,node, k)

Mass in every partition of an HS-Tree is used to profile the characteristics of data. Let Score(x, T ) be a function that traverses a test instance x from the root of an HS-Tree (T ) until a terminal node. This function then returns the anomaly score of x by evaluating Node.r × 2 ^ (Node.k) The final score for x is the sum of scores obtained from each HS-Tree in the ensemble. 

#### 5. UpdateResetModel(node)

At the end of each window, the model is updated. The model update procedure is simple before the start of the next window, the model is updated to the latest mass by simply transferring the non-zero mass l to r

#### 6. StreamingHSTrees(X, window_size, t, h)

It shows the operational procedure for Streaming HS-Trees. It builds an ensemble of Half-Space Trees. It uses the first ψ instances of the stream to record its initial reference mass profile in the HS-Trees. Since these instances come from the initial reference window, only mass rof each traversed node is updated. After these two steps, the model is ready to provide an anomaly score for each subsequent streaming point. Mass r is used to compute the anomaly score for each streaming point. The recording of mass for each subsequent streaming point in the latest window is then carried out on mass l. At the end of each window, the model is updated. The model update procedure is simple before the start of the next window, the model is updated to the latest mass by simply transferring the non-zero mass l to r. This process is fast because it involves no structural change of the model. After this, each node with a non-zero mass l is reset to zero.