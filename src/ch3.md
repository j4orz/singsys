# 2. Generation

**Contents**
- [2.1 Autoregressive Models]()
    - [Recurrent Neural Networks (RNNs)]()
    - [Long Short-Term Memory Networks (LSTMs)]()
    - [Generative Pretrained Transformers (GPTs) with dense qwen/llama]()
    - [Mixture of Experts (MoEs) with moe deepseek/olmoe]()
<!-- - [3.2 Diffusion Models]()
https://x.com/DrYangSong/status/1891314153580556468
https://x.com/dpkingma/status/1863662926101434594
https://x.com/sirbayes/status/1773771737127428236
https://diffusion.csail.mit.edu/
- [3.3 Variational Autocenters]()
- [3.4 Normalizing Flow Models]()
- [3.5 Generative Adversarial Networks]()
- [3.3 Energy Based Models]() -->

# 2.1 Autoregressive Models

## Feedforward Neural Networks (FNNs)

Autoregressive language models.
logistic regression -> FFN -> RNN -> LSTM -> GPT

```python
"""
model: Neural Language Models (Bengio et al. 2003)

Dimension key:
# windows
B: batch size
T: sequence length

# input/output
V: vocabulary size
E: embedding dimension (E != D in paper)
D: model dimension
"""
import picograd
# import torch
import matplotlib.pyplot as plt
# picograd = torch
# %matplotlib inline
# from jaxtyping import

# *********************MODEL*********************
B, T = 32, 3
V, E, D = 27, 10, 200

class Linear:
  def __init__(self, D_in, D_out, bias=True):
    self.W_DiDo = picograd.randn((D_in, D_out)) * 0.01
    self.b_Do = picograd.zeros(D_out) if bias else None

  def __call__(self, X_Di):
    self.X_Do = X_Di @ self.W_DiDo
    if self.b_Do is not None: self.X_Do += self.b_Do
    self.out = self.X_Do
    return self.X_Do

  def parameters(self):
    return [self.W_DiDo] + ([] if self.b_Do is None else [self.b_Do])

class Tanh:
  def __call__(self, X_BD):
    self.X_BD = picograd.tanh(X_BD)
    self.out = self.X_BD
    return self.X_BD
  
  def parameters(self):
    return []

model = [
  Linear(T * E, D, bias=False), Tanh(),
  Linear(D, D, bias=False), Tanh(),
  Linear(D, V, bias=False)
]

C_VE = picograd.randn((V,E)) #, generator=g)
params = [C_VE] + [p for l in model for p in l.parameters()]
for p in params:
    p.requires_grad = True

print("model loaded to cpu")

# *********************DATA*********************
import picograd.nn.functional as F
import random

words = open('./tests/names.txt', 'r').read().splitlines()
v = sorted(list(set(''.join(words))))
encode = { c:i+1 for i,c in enumerate(v) }
encode['.'] = 0
decode = { i:c for c,i in encode.items() }

def gen_dataset(words):
  X, Y = [], []
  for w in words[:10]:
    context = [0] * T;
    for c in w + '.':
      X.append(context)
      Y.append(encode[c])
      # print(''.join(decode[i] for i in context), '-->', decode[encode[c]])
      context = context[1:] + [encode[c]]

  X, Y = picograd.tensor(X), picograd.tensor(Y) # X:(N,C) Y:(N)
  return X, Y

random.seed(42)
random.shuffle(words)
n1, n2 = int(0.8*len(words)), int(0.9*len(words))
X_NT, Y_N = gen_dataset(words)#[:n1])
print(X_NT.shape, Y_N.shape)
Xdev, Ydev = gen_dataset(words[n1:n2])
Xte, Yte = gen_dataset(words[n2:])

# *********************TRAINING LOOP*********************
# 1. preprocess
# 2. forward
# 3. backward
# 4. update

losses, steps = [], []
for step in range(100): #200000):
  # 1. preprocess
  i_B = picograd.randint(0, X_NT.shape[0], (B,)) # a. minibatch: X_NT -> X_BT
  X_BT, Y_B = X_NT[i_B], Y_N[i_B]
  X_BTE = C_VE[X_BT] # b. embed: X_BT -> X_BTE where t ∈ [0,V]
  X = X_BTE.reshape((-1, T * E)) # concat so X=X_B(TE) # TODO: can take .view instead

  # 2. forward
  for h in model:
    X = h(X) # X_B(TE) -> X_BD -> X_BV
  yhat_BV = X
  loss = F.cross_entropy(yhat_BV, Y_B)

  # 3. backward
  for layer in model:
    layer.out.retain_grad() # 6 .retain_grad()
  for p in params:
    p.grad = None
  loss.backward()

  # 4. update
  for p in params:
    p.data += -0.01 * p.grad

  (_, _) = (steps.append(step), losses.append(loss.log10().item()))
  if step % 10000 == 0: print(f"step: {step}/{200000}, loss {loss.item()}")

plt.plot(steps, losses)

# *********************INFERENCE LOOP*********************
for _ in range(20): # n samples
  output, context = [], [0] * T
  while True:
    # 1. preprocessing: C_VE[X_1T]
    # print("tokens", context)
    X_1T = picograd.tensor([context]) # B=1 for inference (1 response)
    X_1TE = C_VE[X_1T] # X_1T ∈ [0..=26] must hold
    X_1cTE = X_1TE.reshape((-1, T*E)) # reshape from 3dims -> 2dims B=1 TE
    X = X_1cTE

    # 2. process: f: ℝ^d -> [0,1]^k
    for h in model:
      X = h(X)
    y_hat = F.softmax(X, dim=1)

    # 3. postprocess: sample + update
    token = picograd.multinomial(y_hat, num_samples=1, replacement=True).item()#, generator=g).item()
    output.append(decode[token])
    context = context[1:] + [token]
    if token == 0:
        break
  print(''.join(output))
```

## Recurrent Neural Networks (RNNs)

## Long Short-Term Memory Networks (LSTMs)

## Generative Pretrained Transformers (GPTs)

- gpt2
- gpt3

## Generative Pretrained Transformers (GPTs++)

- 1. manning ebook-2.torchtitan/torchtune-3.flexattention: llama1, llama2, llama3, llama4
- modded nanogpt by keller. based on llm.c by karpathy.
- beyondnanogpt
