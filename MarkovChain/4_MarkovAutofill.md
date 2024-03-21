# Markov Chain

[ChatGPT](https://chat.openai.com/share/79446670-6fa4-4df1-98ab-e35899250a4d)

Using the idea of Markov chains, write a python function generate(filename: str, start_words: list[str], chain_length: int, num_generated: int) -> str which takes a filename, a chain length, a list of start words which has to be exactly as long as the chain_length (why?), and an integer num_generated and returns a sentence num_generated words long which sounds similar to the text contained in filename.

    A Markov chain is a stochastic model that describes a sequence of possible events in which the probability of each event depends only on the state attained in the previous event. In text generation, a Markov chain can be used to predict the next word in a sequence based on the preceding words.

    Here's a Python function that generates a sentence using a Markov chain based on the text contained in the specified file:

```python
import random

def generate(filename: str, start_words: list[str], chain_length: int, num_generated: int) -> str:
    # Read the text from the file
    with open(filename, 'r', encoding='utf-8') as file:
        text = file.read()
    
    # Tokenize the text into words
    words = text.split()
    
    # Create a dictionary to store the Markov chain transitions
    chain = {}
    for i in range(len(words) - chain_length):
        key = tuple(words[i:i + chain_length])
        value = words[i + chain_length]
        if key in chain:
            chain[key].append(value)
        else:
            chain[key] = [value]
    
    # Generate the sentence
    sentence = start_words.copy()
    while len(sentence) < num_generated:
        key = tuple(sentence[-chain_length:])
        if key in chain:
            next_word = random.choice(chain[key])
            sentence.append(next_word)
        else:
            break
    
    return ' '.join(sentence)

# Example usage
generated_sentence = generate("your_file.txt", ["start", "words"], 2, 10)
print(generated_sentence)

```

Instead of using for i in range, can you use for word in words?

```python
import random

def generate(filename: str, start_words: list[str], chain_length: int, num_generated: int) -> str:
    with open(filename, 'r', encoding='utf-8') as file:
        text = file.read()
    
    # Tokenize the text into words
    words = text.split()
    
    # Create a dictionary to store the Markov chain transitions
    markovs = {}
    # Use a sliding window approach to iterate over the words
    for i in range(len(words) - chain_length):
        key = tuple(words[i:i + chain_length])
        value = words[i + chain_length]
        if key in markovs:
            markovs[key].append(value)
        else:
            markovs[key] = [value]
    
    # Generate the sentence
    sentence = start_words.copy()
    # Use a while loop to generate the sentence
    while len(sentence) < num_generated:
        key = tuple(sentence[-chain_length:])
        if key in markovs:
            next_word = random.choice(markovs[key])
            sentence.append(next_word)
        else:
            break
    
    return ' '.join(sentence)

# Example usage
generated_sentence = generate("your_file.txt", ["start", "words"], 2, 10)
print(generated_sentence)

```

It wasn't able to comprehend, so I decided to modify on my own based on Aruvi's lecture 4.

Then, I asked ChatGPT to generate some testcases. 

It gave me some basics testcases, I have included the same in colab.