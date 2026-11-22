## Marc's Word

**Flag:** `CyberZerk{ESPOIR}`

**Category:** Awareness  
**Difficulty:** Easy  
**Author:** Perceval

### Solution

**1. Analyse the challenge description**

The clues point towards a hidden word:

- "Read between the lines" suggests an acrostic.
- "One word, just one" indicates that the answer is a single word.
- "The word he clung to when everything seemed cracked" suggests hope or resilience.

**2. Find Marc's LinkedIn post**

Source: https://www.linkedin.com/company/cyberzerk

The original French text is required because translating it changes the initial letters and destroys the hidden pattern.

**3. Identify the acrostic**

Take the first letter of each paragraph in Marc's message, stopping before the `---` separator.

| Paragraph | First letter |
|---|---|
| 1. En cybersécurité... | E |
| 2. Se taire... | S |
| 3. Pourtant... | P |
| 4. On se croit seul... | O |
| 5. Il n'y a aucune honte... | I |
| 6. Rappelle-toi... | R |

The resulting letters spell:

`E-S-P-O-I-R`

**4. Validate the answer**

*Espoir* is French for **hope**.

It matches the challenge's description of something to hold on to when everything seems broken.

The `---` separator marks the end of Marc's message. The content below it is unrelated to the acrostic.

### Final Flag

`CyberZerk{ESPOIR}`

### Notes

- The English translation does not preserve the acrostic.
- The poster and its P.S. reference to coefficients are unrelated to this challenge.
- The correct extraction method is the first letter of each paragraph in the original French message.
