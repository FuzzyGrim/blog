---
layout: post
title: "SummonerStats"
date: 2022-12-09
excerpt: "A site that provides League of Legends summoner's stats."
tag:
    - project
    - python
---

<!-- ABOUT THE PROJECT -->

## About The Project

![screenshot](https://github.com/FuzzyGrim/SummonerStats/raw/master/api/static/assets/screenshot.png)

[SummonerStats](https://github.com/FuzzyGrim/SummonerStats) is a site that provides League of Legends summoner's stats.

### Built With

-   [Django](https://djangoproject.com), a high-level Python Web framework.
-   [Bootstrap](https://getbootstrap.com), a free and responsive framework for faster and easier web development.
-   [Sqlite3](https://www.sqlite.org/index.html), a small, fast, self-contained SQL database engine.
-   [JQuery](https://jquery.com), a fast, small, and feature-rich JavaScript library.

<!-- GETTING STARTED -->

## Getting Started

To get a local copy up and running follow these simple example steps.

### Installation

1. Install [Python 3](https://www.python.org/downloads/)
1. Get a free API Key at [Riot Developer Portal](https://developer.riotgames.com/)
1. Clone the repo
    ```sh
    git clone https://github.com/FuzzyGrim/SummonerStats.git
    ```
1. Install Python dependencies
    ```sh
    pip install -r requirements.txt
    ```
1. Create `.env` file and add your API Key
    ```sh
    API = 'ENTER YOUR API';
    ```
1. Generate a Django secret key
    ```sh
    python -c "import secrets; print(secrets.token_urlsafe())"
    ```
1. Add the secret key to the `.env` file
    ```sh
    SECRET = 'ENTER YOUR SECRET';
    ```
1. Apply migrations
    ```sh
    python manage.py migrate
    ```
1. Run server
    ```sh
    python manage.py runserver
    ```
1. Now that the server’s running, visit http://127.0.0.1:8000/ with your Web browser

<!-- LICENSE -->

## License

Distributed under the GPLv3 License. See [`LICENSE`](https://github.com/FuzzyGrim/SummonerStats/blob/master/LICENSE) for more information.
