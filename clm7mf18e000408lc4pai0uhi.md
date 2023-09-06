---
title: "Siamese Networks for One-shot Learning"
seoTitle: "Siamese Networks for One-Shot Learning by Jay Gala"
datePublished: Wed Sep 06 2023 10:52:53 GMT+0000 (Coordinated Universal Time)
cuid: clm7mf18e000408lc4pai0uhi
slug: siamese-networks-for-one-shot-learning
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693997680858/c7efb966-a36c-40d0-a304-9cc0b63ed95b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1693997494567/4946366c-01c2-47c2-b929-9c874862079a.png
tags: ai, machine-learning, deep-learning, pytorch, few-shot-learning

---

I have been working with Siamese Networks a lot in the past few months so I decided to write some articles on it that I would've liked to read the first time I got introduced to the topic. First, we will discuss Siamese networks in general and then how they can be used for one-shot learning tasks like classification.

## What are Siamese Networks?

A Siamese Neural Network (SNN) is a system of two identical / twin neural networks. Thus, the term "Siamese" comes from Siamese Twins. These identical neural networks share the same parameters and are used in parallel to compare between two inputs. Let's see how 👇

[![Few-Shot Learning (2/3): Siamese Networks - YouTube](https://i.ytimg.com/vi/4S-XDefSjTM/maxresdefault.jpg align="left")](https://www.youtube.com/watch?app=desktop&v=4S-XDefSjTM)

Let's say you have 2 images x1 and x2 of a tiger and you pass them as input to your Siamese net. Since these are images of the same animal i.e. same class you'd want their representations to be closer to each other than that of a pigeon. We can then calculate the Euclidean distance between these representations h1 and h2 and store it as another vector z. Then pass it through a sigmoid et voila you now have a probability of whether or not the input images are similar or not. Cool... how can I use this though?

Siamese Networks have many applications like:

1. **Face Verification and Recognition**: Siamese networks can be used for face verification and recognition tasks. Given two face images, the network learns to measure the similarity between them. This is used in applications like unlocking smartphones with facial recognition.
    
2. **Signature Verification**: In the financial industry, Siamese networks can be employed to verify signatures by comparing them with reference signatures. Banks and other financial institutions use this technology to detect fraudulent signatures. ([Original paper](https://proceedings.neurips.cc/paper/1993/file/288cc0ff022877bd3df94bc9360b9c5d-Paper.pdf))
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693993818847/5d0c4c52-9ccc-4068-a7e9-6b5a29c3d499.png align="center")
    
3. **Image Retrieval**: In content-based image retrieval systems, Siamese networks can be used to find images that are similar to a query image. This is useful in image search engines and recommendation systems.
    

## Training your Siamese Net

Training your Siamese network is as easy as training any other network using any high-level deep learning library like TensorFlow, PyTorch, etc. You just need to know about the different loss functions because in SNNs we don't measure how accurate the network is in predicting a class, instead, we measure how accurate the network is in modeling a similarity or dissimilarity between a pair of inputs.

Popular loss functions include:

1. **Contrastive Loss**: Encourages similarity between similar pairs and dissimilarity between dissimilar pairs.
    

$$L = (1 - Y) \cdot \frac{1}{2}(D)^2 + Y \cdot \frac{1}{2} \max(0, \text{margin} - D)^2$$

* Where:
    
    * ***L*** is the contrastive loss.
        
    * ***Y*** is the binary label indicating whether the pair is similar (0 for dissimilar, 1 for similar).
        
    * ***D*** is the distance or dissimilarity score between the pair.
        
    * **margin** is a hyperparameter that defines the minimum dissimilarity score that the network should aim for between dissimilar pairs.
        

1. **Triplet Loss**: Focuses on triplets of data points (anchor, positive, negative) and encourages positive pairs to be closer and negative pairs to be farther apart.
    

$$L = \max(D(\text{anchor}, \text{positive}) - D(\text{anchor}, \text{negative}) + \text{margin}, 0)$$

* Where:
    
    * ***L*** is the triplet loss.
        
    * ***D*(*x*,*y*)** represents the distance between embeddings of samples ***x*** and ***y***.
        
    * The **margin** is a hyperparameter that defines the minimum difference that the network should aim for between positive and negative pairs.
        

Starter code in PyTorch (written by chatGPT, verified by me):

```python
# Siamese Network Architecture
class SiameseNetwork(nn.Module):
    def __init__(self):
        super(SiameseNetwork, self).__init__()
        self.cnn = nn.Sequential(
            nn.Conv2d(1, 32, 5),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2,2),
            nn.Conv2d(32, 64, 5),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(2,2)
        )
        self.fc = nn.Sequential(
            nn.Linear(64*53*53, 256),
            nn.ReLU(inplace=True),
            nn.Linear(256, 256),
            nn.ReLU(inplace=True),
            nn.Linear(256, 128)
        )
        
    def forward_one(self, x):
        x = self.cnn(x)
        x = x.view(x.size()[0], -1)
        x = self.fc(x)
        return x

    def forward(self, input1, input2):
        output1 = self.forward_one(input1)
        output2 = self.forward_one(input2)
        return output1, output2

# Contrastive Loss
class ContrastiveLoss(nn.Module):
    def __init__(self, margin=2.0):
        super(ContrastiveLoss, self).__init__()
        self.margin = margin

    def forward(self, output1, output2, label):
        euclidean_distance = F.pairwise_distance(output1, output2, keepdim=True)
        loss_contrastive = torch.mean((1-label) * torch.pow(euclidean_distance, 2) +
                                      (label) * torch.pow(torch.clamp(self.margin - euclidean_distance, min=0.0), 2))
        return loss_contrastive

model = SiameseNetwork()

criterion = ContrastiveLoss()

optimizer = optim.SGD(model.parameters(), lr=0.01)

# Training loop
for epoch in range(num_epochs):
    for i, (input1, input2, label) in enumerate(train_loader):
        optimizer.zero_grad()
        output1, output2 = model(input1, input2)

        # label is either 1 or 0 indicating similar or dissimilar pair 
        loss = criterion(output1, output2, label)
        loss.backward()
        optimizer.step()
```

## One-Shot Image Recognition

Now that you've learned how to train an SNN, we can look into one-shot image classification using SNNs. ([Original Paper](https://www.cs.cmu.edu/~rsalakhu/papers/oneshot1.pdf))

Performing one-shot recognition with a trained Siamese network involves comparing a new query instance (or test example) against a set of reference instances (one support example per class) to determine if the query instance is similar to any of the reference instances.

Example: Assume an office building where employees are allowed entry via facial verification. In this problem, the support instances are one-shot examples (since you don't have millions of images of that person) of employees. When a person enters (the query instance) the building, you take a picture of them and embed it using the trained Siamese network, calculate the similarity with all the existing employee embeddings, and compare it against a threshold. If the similarity score with any employee's face exceeds the threshold, you recognize the person; otherwise, it's an unknown face.

**Note:** In these types of problems, the Siamese network might need to be trained on similar-looking images i.e. human faces in this case, and not trains or trucks. This helps the network to learn the distinguishing features of a human face and what sets two persons apart.

Starter code in PyTorch (written by chatGPT, verified by me):

```python
def one_shot_recognition(query_image_path, reference_embeddings):
    query_image = load_image(query_image_path)
    query_embedding = siamese_net.forward_one(query_image)

    for i, reference_embedding in enumerate(reference_embeddings):
        similarity_score = calculate_similarity(query_embedding, reference_embedding)
        if similarity_score > similarity_threshold:
            print(f"Recognized as reference image {i}")
            return

    print("No match found")
```

### Additional resources

Siamese Neural Networks for One-shot Image Recognition: [Link](https://www.cs.cmu.edu/~rsalakhu/papers/oneshot1.pdf)

[https://medium.com/@rinkinag24/a-comprehensive-guide-to-siamese-neural-networks-3358658c0513](https://medium.com/@rinkinag24/a-comprehensive-guide-to-siamese-neural-networks-3358658c0513)

[https://pyimagesearch.com/2020/11/30/siamese-networks-with-keras-tensorflow-and-deep-learning/](https://pyimagesearch.com/2020/11/30/siamese-networks-with-keras-tensorflow-and-deep-learning/)

[https://youtu.be/6jfw8MuKwpI?si=8OQfKUo-zzRcTbt8](https://youtu.be/6jfw8MuKwpI?si=8OQfKUo-zzRcTbt8)

[https://www.youtube.com/watch?v=d2XB5-tuCWU](https://www.youtube.com/watch?v=d2XB5-tuCWU)