# MkDocs

Parents: [[01_Tools]], [[writing]]
See also: [[diataxis]]


A static site generator from Markdown documents, for technical documentation.

You install it with pip, create a new project with `mkdocs new project_name`, then edit `mkdocs.yml` to control the output. The site can be seen locally with `mkdocs serve`. All Markdown documents go in `docs/`. For new pages, create a Markdown + add info to the Yaml file (including the position of this file in documentation). There are themes (controlled in the same Yaml, in the header; try `theme: readthedocs` for example).

Once done, build the site with `mkdocs build`, if you wanna to host htmls somewhere.

Can be hosted on [[github]] Pages, apparently (https://pages.github.com/)

# Refs

https://www.mkdocs.org/