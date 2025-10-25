## Link:  https://vail-ucla.github.io/


## Setup

1. setup docker engine in you local machine. [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/).
2. run the following command that will pull the latest pre-built image from DockerHub and will run your website.

```bash
$ docker compose pull
$ docker compose up
```


## How to Update

* Update News: [_news/](_news/)
    * For awards, please follow the format *XXX (with personal link) won XXXXX price*.
    * For paper acceptance, please follow the format *AA, BB, and CC were accepted to XXXX*.
    * For workshop, please follow the format *We organized YYYYY workshop at XXXXX*.




* Update Publications: [_bibliography/papers.bib](_bibliography/papers.bib)
    *  Add the field `selected={true}` if Bolei wants it to appear in the selected publication list
    *  Use the field `tags={XXX}` to add your paper to the corresponding category
    *  Existing categories are: `Computer Vision, Robotics, Simulation, Autonomy, Generative Models, Others`
    * Use the field `teaser={XXXX}` to add your teaser image, upload your teaser image to [assets/teaser](assets/teaser)
* Update teaser video: `highlighted_projects` in [_pages/main.md](_pages/main.md)
    * Upload your video to: [assets/video/](assets/video/)
    * For teaser video, make sure the length is **5 seconds tight** 

* Update Team Members: [_data/team.yml](_data/team.yml)
    * Team member photo folder: [assets/team/](assets/team/)
    * Use existing template for the `role` field
    * For all non-alumni members, the `affiliation` field should **always** be `UCLA`
    * Make sure to update your personal webpage link! If you don't have a personal webpage, then please use your LinkedIn address.