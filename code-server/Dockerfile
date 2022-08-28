# base image
FROM linuxserver/code-server:v4.0.2-ls111

# install updates
RUN apt-get update --quiet && apt-get upgrade --yes --quiet

# install nodejs, npm amd yarn
RUN set -ex; \
  curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add; \
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list; \
  apt-get update --quiet && apt-get install --yes --quiet yarn; \
  node --version; \
  npm --version; \
  yarn --version

# install pnpm
RUN npm install -g pnpm

# add ssh key to access github
RUN mkdir -p /config/.ssh
RUN --mount=type=secret,id=CODE_SERVER_PRIVATE_SSH_KEY_GITHUB cat /run/secrets/CODE_SERVER_PRIVATE_SSH_KEY_GITHUB > /config/.ssh/id_rsa
RUN chmod 600 /config/.ssh/id_rsa

# install fish shell and customize the theme
RUN apt-get install --yes --quiet fish

# apply customizations to fish theme
COPY ./config/fish_prompt.fish /usr/share/fish/functions/
COPY ./config/fish_right_prompt.fish /usr/share/fish/functions/

# install extensions (look for other extensions on https://open-vsx.org/)
RUN set -ex; \
  code-server  --extensions-dir=/config/extensions \
    --install-extension redwan-hossain.auto-rename-tag-clone \
    --install-extension mgmcdermott.vscode-language-babel \
    --install-extension wesbos.theme-cobalt2 \
    --install-extension vivaxy.vscode-conventional-commits \
    --install-extension hediet.vscode-drawio \
    --install-extension dsznajder.es7-react-js-snippets \
    --install-extension dbaeumer.vscode-eslint \
    --install-extension mhutchie.git-graph \
    --install-extension heybourn.headwind \
    --install-extension bierner.markdown-preview-github-styles \
    --install-extension PKief.material-icon-theme \
    --install-extension anwar.papyrus-pdf \
    --install-extension esbenp.prettier-vscode \
    --install-extension humao.rest-client \
    --install-extension bradlc.vscode-tailwindcss \
    --install-extension Gruntfuggly.todo-tree \
    --install-extension redhat.vscode-yaml \
    --install-extension silvenon.mdx \
    --install-extension prisma.prisma

# install font (https://github.com/coder/code-server/issues/1374, https://github.com/tonsky/FiraCode)
COPY ./fonts /usr/local/share/.config/yarn/global/node_modules/code-server/vendor/modules/code-oss-dev/out/vs/code/browser/workbench/fonts
RUN sed -i "s|<head>|\
  <style> \n\
    @font-face { \n\
      font-family: 'Cascadia  Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-Light.woff2') format('woff2'); \n\
      font-weight: 300; \n\
      font-style: normal; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia  Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-LightItalic.woff2') format('woff2'); \n\
      font-weight: 300; \n\
      font-style: italic; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-Regular.woff2') format('woff2'); \n\
      font-weight: 400; \n\
      font-style: normal; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-Italic.woff2') format('woff2'); \n\
      font-weight: 400; \n\
      font-style: italic; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-SemiBold.woff2') format('woff2'); \n\
      font-weight: 600; \n\
      font-style: normal; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/CascadiaCodePL-SemiBoldItalic.woff2') format('woff2'); \n\
      font-weight: 600; \n\
      font-style: italic; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/woff2/CascadiaCode-Bold.woff2') format('woff2'); \n\
      font-weight: 700; \n\
      font-style: normal; \n\
    } \n\
    @font-face { \n\
      font-family: 'Cascadia Code'; \n\
      src: url('static/out/vs/code/browser/workbench/fonts/woff2/CascadiaCode-BoldItalic.woff2') format('woff2'); \n\
      font-weight: 700; \n\
      font-style: italic; \n\
    } \n\
  \n\</style><head>|g" /usr/local/share/.config/yarn/global/node_modules/code-server/vendor/modules/code-oss-dev/out/vs/code/browser/workbench/workbench.html

# copy settings file
RUN mkdir -p /config/data/User
COPY ./config/settings.json /config/data/User/settings.json

# set git config
RUN set -ex; \
  git config --global user.name "<GITHUB_USERNAME>"; \
  git config --global user.email "<ME@EMAIL.COM>"