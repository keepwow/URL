# Nautical Log

1. The project starts from the original `docs/index.md`.

    ```markdown
    # Welcome to MkDocs

    For full documentation visit [mkdocs.org](https://www.mkdocs.org).

    ## Commands

    * `mkdocs new [dir-name]` - Create a new project.
    * `mkdocs serve` - Start the live-reloading docs server.
    * `mkdocs build` - Build the documentation site.
    * `mkdocs -h` - Print help message and exit.

    ## Project layout

        mkdocs.yml    # The configuration file.
        docs/
            index.md  # The documentation homepage.
            ...       # Other markdown pages, images and other files.

    ```

1. Use [material](https://squidfunk.github.io/mkdocs-material) theme for its multi-language support.

    - Add [plugins/privacy](https://squidfunk.github.io/mkdocs-material/plugins/privacy/) settings to speed up loading the pages with web fonts [self-hosted](https://squidfunk.github.io/mkdocs-material/setup/changing-the-fonts/) in the site directory rather than load from Google.

1. I set up a nginx reverse proxy server, which listens to port `8000`, served by the `mkdocs serve -v` command in the root direcotry of this project, and of course, I modified the `/etc/hosts` to suit the case. Therefore, I can focus on writing the markdown files without concerning about when to run the `mkdocs build` command.

    - Ningx conf:

        ```conf
        server {
            listen 80;
            server_name URL.book;

            location / {
                proxy_pass http://127.0.0.1:8000;
            }

            # location / {
            #     root   /Users/m/web/html/URL/site;
            #     index  index.html index.htm;
            # }
        }
        ```

    - Append `127.0.0.1 URL.book` to `/etc/hosts`
