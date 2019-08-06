# Tips and Tutorials
Tips and tutorials by @dalanzg

Developed with [Hugo](https://gohugo.io/).

[https://dalanzg.github.io/tips-tutorials/](https://dalanzg.github.io/tips-tutorials/)

## GitHub configuration

2 branches to separate source code and published site:
- **master**: source code
- **gh-pages**: published site

## Install Dependencies
Feel free to fork my GitHub repository.

Install the following dependencies:
- **nodejs**
- **hugo**

## Workflow
Install **node_modules** dependencies:

```terminal
$ npm install
```

Run **npm-scripts**:
- `run npm build`: Copy files from node_modules and build hugo project (build **docs** folder)
- `run npm serve`: Serve Hugo project
