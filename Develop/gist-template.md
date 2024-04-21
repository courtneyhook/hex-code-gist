# Matching a Hex Value using a Regex

A hex value is a color code that is used to describe a specific color in RGB format. It begins with a pound sign (#) and is typically followed by 6 characters. The allowed characters are a, b, c, d, e, f, 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9.

The first two characters after the pound sign represent the amount of red. The second set of two characters represent the amount of green, and the last two characters represent the amount of blue. The following is an example of a hex code:

<code>#<span style="color:red">7d</span><span style="color:green">6b</span><span style="color:blue">91</span></code>

## Summary

In this gist, I will be explaining regex used to find a hex code :

> /^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/

## Table of Contents

- [Literal and Meta-Characters](#literal-and-meta-characters)
- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Character Classes](#character-classes)
- [OR Operator](#or-operator)
- [Bracket Expressions](#bracket-expressions)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Back-references](#back-references)
- [Boundaries](#boundaries)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

A regular expression, or regex for short, is a searchable pattern that can look for specific patterns in your code. It can help find all phone numbers, emails, urls, or other formatted patterns. It can also find code that follows a specific pattern, such as every three letter word, all words that start with 'c' or end with 's', or words that contain capital letters. A regex is set up to search for the specific format you are looking for.

The regex that searches for a hex code looks like this:<br>

> /^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/

---

### Literal and Meta-Characters

The first idea to understand is literal and meta characters. A literal character is the exact character you are searching for. In the case of the hex code, it always starts with the pound sign, so we will need to search for a _literal_ pound sign. To search for a literal character, you will simply use the actual character you are searching for. /^<span style="color:red">**#**</span>([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/

A meta character does not mean the actual character, but instead it stands for a generalized pattern. The common meta characters are as follows:

| Meta Character | Meaning                                           |
| -------------- | ------------------------------------------------- |
| \d             | any digit 0 - 9                                   |
| \w             | any character A - Z, a - z, 0 - 0                 |
| \s             | any whitespace                                    |
| .              | any character                                     |
| \D             | any character that is **NOT** a digit 0 - 9       |
| \W             | any character that is **NOT** A - z, a - z, 0 - 9 |
| \S             | any character that is **NOT** whitespace          |

These characters search for a general type of character instead of a specific character.

---

### Anchors

Anchors show position in the regex. Two anchors are ^ and \$. You use a carrot(^) at the beginning to show the beginning of the expression. You will use a dollar sign ($) at the end to show the end of the expression.

In the hex code example, the expression must start with a pound sign and end after 6 or 3 characters.

> /<span style="color:red">^</span>#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})<span style="color:red">$</span>/

---

### Quantifiers

Quantifiers are a type of meta-character that changes the characters before it. Here are a few examples of common quantifiers and their meanings.

| Quantifier  | Meaning                                       |
| ----------- | --------------------------------------------- |
| \*          | 0 or more characters                          |
| +           | 1 or more characters                          |
| ?           | 0 or 1                                        |
| { min, max} | set the minimun quantity and maximum quantity |
| { n }       | a specific number                             |

To illustrate this, we can look at an example. \d\d\d represents three digits in a row. Instead of using the same meta character three times, we can use a quantifier to shorten our expression. \d{3} would be the same as \d\d\d. {3} is a quantifier that says find three of the previous characters.

In the hex code example, there are two quantifiers.

> /^#([A-Fa-f0-9]<span style="color:red">{6}</span>|[A-Fa-f0-9]<span style="color:red">{3}</span>)$/

Typically a hex code has six characters, so the first quantifier is {6}. Optionally, when the first two characters are the same, the second two characters are the same and the last two characters are the same, it can be shortened to use only 3 characters. An example would be <span style="background-color:#aa55dd">#aa55dd</span>. This code could be shortened to <span style="background-color:#a5d">#a5d</span>. Because of this option, the second quantifier is {3} to catch these exceptions.

---

### Greedy and Lazy Match

When using the quantifiers <span style="background-color:gray">&nbsp; .\* &nbsp;</span> and <span style="background-color:gray">&nbsp;.+&nbsp;</span>, the search will try to match as many characters as possible that fit the regex. The search does not end until the end of the line. This is called a greedy quantifier. When we use a dot followed by &nbsp;\*&nbsp; or &nbsp;+&nbsp;, it is translated as find every character. Given the line:

"...a dot(.) followed by a star (\*) or a plus (+)..."

We could try to search for everything inside the parenthesis by using <span style="background-color:gray">\(&nbsp;.\*&nbsp;)</span>. This would work if there was only one set of parenthesis on the line. Because there are multiple sets of parenthesis, the search would return:

_(.) followed by a star (\*) or a plus (+)_

This is because <span style="background-color:gray">\(&nbsp;.\*&nbsp;)</span> is greedy and it tried to match as much as it could. In order to limit the matches, we must make the quantifier lazy by adding a question mark to our expression. It would look like <span style="background-color:gray">\(&nbsp;.\*?&nbsp;)</span>. This would return all three sets of parenthesis and their contents separately.

This concept is not used in our hex code regex since we are looking for a specific number of characters.

---

### Character Classes

Character classes are sets of characters that can be matched in a regex. They are contained inside a set of brackets and hold possible characters that can be used. There is an example of character classes in the hex code example.

> /^#(<span style="color:red">[A-Fa-f0-9]</span>{6}|<span style="color:red">[A-Fa-f0-9]</span>{3})$/

Inside the brackets are the characters A - Z a - z 0 - 9. This means that the only characters that can be matched are A, B, C, D, E, F, a, b, c, d, e, f, 0, 1, 2, 3, 4, 5, 6, 7, 8 or 9. Any other character will not be considered for this search.

You can also use character classes to give options for spellings. [CK]ourtney would find Courtney or Kourtney.

---

### OR Operator

The OR Operator is a way of giving options with which to search. It is defined with a set of parenthesis with the pipe symbol separating the options. In the hex code example, the code can be 6 or 3 characters long. The option <code>>[A-Fa-f0-9]{6}</code> says to look for 6 characters listed and the option <code>[A-Fa-f0-9]{3}</code> says to look for 3 characters listed. We do not want 9 characters altogether. We either want 6 **_OR_** 3. To show this, we put both options inside parenthesis and separate them with the pipe symbol:

<span style="color:red">(</span>[A-Fa-f0-9]{6}<span style="color:red">|</span>[A-Fa-f0-9]{3}<span style="color:red">)</span>

---

### Bracket Expressions

We have been using bracket expressions in our hex code example. Bracket expressions are when characters are listed inside brackets. The inside characters are the possibilities of characters to be matched. For example, <code>[abcd123]</code> is a bracket expression with the possible options to match as a, b, c, d, 1, 2, or 3. On the other hand, you can use bracket expressions to tell options not to match. This is accomplished by preceding the options with a carrot(^). An example of this would be <code>[^abcd123]</code>. This means that everything EXCEPT a, b, c, d, 1, 2, or 3 will be matched.

---

### Flags

Flags are optional parameters that modify the search. The are individual letters that are used at the end of the regex. The following table shows the flags and their meanings:

| Flag | Meaning                                                            |
| ---- | ------------------------------------------------------------------ |
| g    | Global search--this searches for all occurances                    |
| i    | Ingore case--this will search for capital and lowercase occurances |
| m    | Multiline--Allows the start (^) and end ($) search across lines    |
| s    | Dot All--Allows the dot(.) to search across lines                  |
| u    | Unicase--treats the sequence as Unicode characters                 |
| y    | Sticky--starts the search from the current position                |

The hex code regex does not include any flags.

---

### Grouping and Capturing

Grouping and capturing is a way of collecting and saving the information in a regex expression and manipulating or updating the expression. As it is, the regex will group the expression together and allow it to be replaced or changed as a whole. However, a user can further manipulate the expression by creating a more detailed regex.

To further explain grouping and capturing, we will need to look at a different regex using a hex code value. We know that all hex codes start with the pound sign followed by 6 word characters. The 6 characters are grouped by two's to represent red, green, and blue. The first two characters are the red values, the third and fourth characters are the green values and the fifth and sixth characters are the blue values. We could group the new regex according to the color values and capture them separately. It would look like this:

^#(\w{2})(\w{2})(\w{2})$

Each set of parenthesis captures a new group. The full expression is considered group 0, the first set of parenthesis is group 1, the second set of parenthesis is group 2 and the third set of parenthesis is group 3. Now that we have grouped and captured the values separately, we can use the groups to manipulate the characters.

---

### Back-references

Back references are a part of grouping and capturing. This is a way to look for repeated expressions. There is no example of this in our hex code example.

---

### Boundaries

A regex looks for a given sequence. Let's say we want to search for the word "not" in the following line:

This note will not be used for advanced notice.

The regex would find "not" in the following places:

This <span style="color:red">not</span>e will <span style="color:red">not</span> be used for advanced <span style="color:red">not</span>ice.

If we want to limit the search to just a single word with no extra characters, we will need to add a word boundary to our regex. It would now be \bnot\b. Adding \b before and after your search limits the search to a single word.

The hex code regex does not use

---

### Look-ahead and Look-behind

The Look-around methods can be used in a search to find a pattern **_ONLY_** if it is preceded or followed by a given pattern.

For example, we want to find the last word in each sentence. We could use a regex to find a word **_ONLY_** if it is followed by a period, question mark or exclamation point. This would use a look-ahead <code>(?=)</code> option and only return words with punctuation.

If we want to find dollar amounts, we could search for numbers **_ONLY_** if the are preceded by a dollar sign. This would be a look-behind <code>(?<=)</code> example.

This is not used in the hex code example.

---

---

### Author

My name is Courtney Hook and I am a third grade teacher. I am studying coding to help make learning accessible to all children. You can find more of my work on my [GitHub](https://github.com/courtneyhook).
