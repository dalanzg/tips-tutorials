# Tips and Tutorials

Tips and tutorials by @dalanzg

Developed with [Hugo](https://gohugo.io/).

[https://dalanzg.github.io/tips-tutorials/](https://dalanzg.github.io/tips-tutorials/)

Feel free to fork my GitHub repository.

## GitHub configuration

2 branches to separate source code and published site:

- **master**: source code
- **gh-pages**: published site

## Build and deployment

Install the following dependencies in your local machine:

- **nodejs**
- **hugo**

Install node_modules:

```terminal
$ npm install
```

Run the following scripts to build and serve hugo site:

- `run npm build`: Copy files from node_modules and build hugo project (build **public** folder)
- `run npm serve`: Serve Hugo project

Finally, commit and push changes to gh-pages branch (publish_to_ghpages.sh)

```terminal
$ rm -rf public
$ git worktree add -B gh-pages public origin/gh-pages
$ run npm build
$ cd public && git add --all && git commit -m "Publishing to gh-pages" && cd ..
$ git push origin gh-pages
```
