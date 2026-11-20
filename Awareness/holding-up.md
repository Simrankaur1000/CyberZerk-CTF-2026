# Holding Up

**Challenge title:** Tenir le Coup  
**Category:** Awareness  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## File

- `recit.txt`

## Solution

### Step 1: Read the text carefully

The file contains a short story about Marc going through difficult weeks.

The hint suggests that the text carries a message "in the background" addressed to him.

There is no complicated encoding involved. The hidden message is contained in the structure of the text itself.

### Step 2: Examine the first letter of each line

The file contains 12 lines. Taking the first character of each line produces:

```text
T
U
N
E
S
P
A
S
S
E
U
L
```

Combining these letters gives:

```text
TUNESPASSEUL
```

This is an **acrostic**, a technique in which selected letters from a text reveal a hidden message.

The message means "Tu n'es pas seul" — "You are not alone."

### Step 3: Extract the message using Python

```python
lines = open("recit.txt", encoding="utf-8").read().strip().split("\n")
print("".join(line[0] for line in lines))
```

Output:

```text
TUNESPASSEUL
```

### Step 4: Format the flag

Wrap the extracted message in the challenge's flag format.

## Flag

`CyberZerk{TUNESPASSEUL}`

## Security Lesson

A message can be hidden in the structure of a text rather than its encoding. Techniques such as acrostics demonstrate why reading carefully matters.

The message is equally important: asking for help is not a weakness, and someone experiencing difficulties may need a colleague to reach out first.
