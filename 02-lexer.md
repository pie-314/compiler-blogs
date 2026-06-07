# what is lexer ?

A lexer is a component that converts raw source code characters into tokens for the parser.

when compiler gets

```text
    .
    .
    total = price + 42;
    .
    .
```

It doesn't know what it is ? for the compiler it's just stream of characters, without any classification

`'t' 'o' 't' 'a' 'l' ' ' '=' ' ' 'p' ...`

now we have two options, either we make meaning out of this now or during later steps add a extra hurdle to handle each character.

If you are the smart one (unlike me), you will think that handling each character later separately is tedious, so we should handle it now and set some rules about what is allowed and what is not.

Lexer is this small set of instructions which can classify a stream of characters into tokens, not to confuse it with grammar, lexer doesn't know rights and wrongs of language.

if take example of standard english.
`jump3d D0g m004. ov3r`

The lexer knows that according to the rules we defined, words can't have numbers, so it can't classify this as a valid word.

and if we write
`jumped Dog moon. over`

The lexer knows this matches the patterns of valid words so it will make `Tokens` out of this. It still doesn't know if sentence is right grammatically, it doesn't know rules of grammar, it just tries to classify stream of characters **for each word.**

>numbers in identifiers are valid in many languages, the above analogy is just an example to understand the logistics of a language.

These meaningful words in a programming language are called Tokens.

### what is a Token ?

Smallest individual element of a program is called as Token. Most things you see inside a program are tokens.

Usually compilers follow the format TokenType(Token) for representation

so `total = price + 42` becomes

```text
[IDENT(total), ASSIGN(=), IDENT(price), PLUS(+),NUMBER(42)]
```

> spaces have no value here, there are languages like python which works on indentations and they follow slightly different rules.

So if we take example of

```text
fn add(a,b){
    price = 10;
    total = price + 42;
    print(total);
}
```

Here the lexer will give output as

```text
[
    FUNCTION(fn),
    IDENT(add),
    LPAREN((),
    IDENT(a),
    COMMA(,),
    IDENT(b),
    RPAREN()),
    LBRACE({),

    IDENT(price),
    ASSIGN(=),
    NUMBER(10),
    SEMICOLON(;),

    IDENT(total),
    ASSIGN(=),
    IDENT(price),
    PLUS(+),
    NUMBER(42),
    SEMICOLON(;),

    IDENT(print),
    LPAREN((),
    IDENT(total),
    RPAREN()),
    SEMICOLON(;),

    RBRACE(})
]
```

Below is example of Toy lexer

```python
source = """
total = price + 42;
"""

tokens = []

i = 0
while i < len(source):
    ch = source[i]

    # Skip whitespace
    if ch.isspace():
        i += 1
        continue

    # Identifier
    if ch.isalpha() or ch == "_":
        start = i

        while i < len(source) and (source[i].isalnum() or source[i] == "_"):
            i += 1

        value = source[start:i]
        tokens.append(("IDENT", value))
        continue

    # Number
    if ch.isdigit():
        start = i

        while i < len(source) and source[i].isdigit():
            i += 1

        value = source[start:i]
        tokens.append(("NUMBER", value))
        continue

    # Operators
    if ch == "=":
        tokens.append(("ASSIGN", ch))
    elif ch == "+":
        tokens.append(("PLUS", ch))
    elif ch == ";":
        tokens.append(("SEMICOLON", ch))
    else:
        tokens.append(("UNKNOWN", ch))

    i += 1

for token in tokens:
    print(token)
```

