# marp-ci

## User

- Create markdown file for marp.
  - Directory : `presentation/pdf`
- Create a branch
  - Branch : `create-pdf/*`

## Developer

- Extention Visual Studio Code.

  - [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)

- Sample
  - slide.md

## Other

- Preview

  - Press `Open Preview to the Side` on Visual Studio Code.

- Build bia docker
  - Image : [marpteam/marp-cli](https://hub.docker.com/r/marpteam/marp-cli/)
  - Build Command : `docker run --rm --init -v $PWD:/home/marp/app/ -e LANG=$LANG marpteam/marp-cli slide.md --pdf`
