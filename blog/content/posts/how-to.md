---
title: "How To"
date: 2022-11-30T17:38:26-05:00
draft: false
---

Next October, the chances of me remembering how to use this
are pretty slim.  So:

- look to see if I have the repo github.com/danroscigno/darvas cloned to
`/home/droscigno/GitHub/darvas/`
  - `git pull` if it is there, or clone it if it is missing
- Start a Golang container (I don't have any luck with Golang, so I run it in a container.  Same with Python.  Probably has nothing to do with my ability to run these, and everything to do with incompatible versions.  I know that Golang version X works with Hugo, so I run the proper version in my container...)

  Note: make sure that the path for the volume (`-v /home...`) is correct

  Note: make sure the UID and GID (`1000:1000`) are also correct

  ```bash
  docker run \
    --env GOCACHE=/go/src/.cache -it \
    --rm \
    --user 1000:1000 \
    --network="host" \
    -v /home/droscigno/GitHub/darvas/src:/go/src \
    --name go golang:1.18 bash
   ````
- open a new shell on the host machine (maybe you can edit the post in the container
but I like my Neovim env on my laptop)
- Navigate to the dir that is mounted in the `-v` above
- Run the `hugo` command to create a post.  What you are reading now was created with:
  ```bash
  cd /home/droscigno/GitHub/darvas/src/quickstart
  hugo new posts/how-to.md
  vi /home/droscigno/GitHub/darvas/src/quickstart/content/posts/how-to.md
  ```
- Run Hugo in dev mode to display the post in a browser at http://localhost:1313/
  ```bash
  hugo server -D
  ```
- Publish
  - Edit the frontmatter of the post and set `draft: false`
  - create a branch if you have not, `git switch -c add-post`
  - Commit the changes and push
    ```bash
    git add .
    git commit -m "add post"
    git push
    ```
  - Create a PR
  - As soon as you create a PR you should see an email from Netlify
  - Once Netlify does its thing the PR will show the link for the preview.  If all you see is the previous post, then double-check the markdown and make sure `draft: false` is there.  Don't ask me how I know that this happens ;)
  - Merge the PR and then the changes will propagate to darvas.roscigno.com
