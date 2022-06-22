## Financial Risk Management using R

### Introduction

I will show how to calculate the return of a portfolio of securities as well as quantify the market risk of that portfolio. I will use the two main tools for calculating the market risk of stock portfolios: Value-at-Risk (VaR) and Expected Shortfall (ES).

### Required libraries

```` markdown
library(quantmod) 
library(moments)
library(MASS)
library(metRology)
library(rugarch)
````

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Roadmap

See the [open issues](https://github.com/evanca/quick-portfolio/issues) for a list of proposed features (and known issues).
___

### References

Financial Risk Management with R, Duke University
