# Personal blog sources

[:house_with_garden:](#)

## Quick Info
 
:construction_worker: built with [hexojs](https://github.com/hexojs/hexo).

:art: theme by [hexo-theme-next](https://github.com/iissnan/hexo-theme-next)

:dancer: special guest: [Live 2D Widget](https://github.com/stevenjoezhang/live2d-widget)


## What can you find here

- main package sources
- theme-next package sources (July 2020)
- theme-next translation to Bosnian + iconized version.
- raw blog data (.md posts and pages)
- live 2d .json

## What you need to make it work

### hexoJS

#### Requirements for hexo

- [Node.js](http://nodejs.org/) (min. version 8.10, recommended 10.0 or higher)
    - [official installers](https://nodejs.org/en/download/)
    - [official guides](https://nodejs.org/en/docs/guides/)
    - [nvm](https://github.com/nvm-sh/nvm)
- [Git](http://git-scm.com/)
    - Win: download & install [git](https://git-scm.com/download/win).
    - Mac: use Homebrew, MacPorts or installer.
    - Debian: `sudo apt install git-core`
    - CentOS: `sudo yum install git-core`
    - Arch: should be preinstalled, if not `sudo pacman -Syu git`

#### hexo installation & Baby steps

*To install hexo run:*

```
npm install --global hexo-cli
```
or
```
npx hexo-cli
```
or for advanced usage:
```
npm install hexo
```

*Basic commands:*

to install hexo and default theme to folder:
```
hexo init <folder> 
```
to specify config file instead using _config.yml:
```node
hexo --config 
```
to install node-modules from `packages.json:
cd <folder>
npm install //or yarn
```
:sos: always handy:
```
hexo --help
```


#### hexo-theme-next

##### Installation and configuration


```
cd <folder-where-u-init-hexo>
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

Use your fav :pencil: editor to change `theme:[]` section in main `_config.yml`

```yml
## Themes: https://hexo.io/themes/
theme: next
```

Edit `_config.yml` in `themes/next/` folder as you prefer, more info on theme [repo](https://github.com/next-theme/hexo-theme-next).


### Run, generate and deploy

:wrench:  To build static files and database: 

- run `hexo generate`
  - (it generates files in site folder, upload it to live server and voila)


:shower:  To erase static files and database: 

- run `hexo clean`
  - (erases node cache and generated files)


:cloud: To deploy:

- run `hexo deploy`
  - (needs deploy server to be configured in `_config.yml`)


:traffic_light:  To serve site on `http://localhost:3000`:

- run `hexo server`
  - (with `--debug` to display all verbose messages in the terminal)


### Change language

In main `_config.yml` change `#Site :arrow_forward: language:[]` section to fav :crossed_flags: language.

To modify language strings go to `themes/next/languages/` and edit .yml file.
