<p align="center">
    <a href="https://legacy-roleplay.com" target="blank">
        <img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/logo.png" height="150px" width="150px" alt="Legacy RP Logo" />
    </a>
</p>

<h1 align="center">
    legacy-rp-admin-v3
</h1>

<p align="center">
    <a href="https://travis-ci.com/ExpDev07/legacy-rp-admin-v3" target="blank">
        <img src="https://travis-ci.com/ExpDev07/legacy-rp-admin-v3.svg?branch=master" alt="Build Status">
    </a
    <a href="https://github.com/ExpDev07/legacy-rp-admin-v3/blob/master/LICENSE.md" target="blank">
        <img src="https://img.shields.io/github/license/ExpDev07/legacy-rp-admin-v3" alt="GitHub">
    </a>
    <a href="https://github.com/ExpDev07/legacy-rp-admin-v3/pulls" target="blank">
        <img src="https://img.shields.io/github/issues-pr/ExpDev07/legacy-rp-admin-v3" alt="GitHub pull requests">
    </a>
    <a href="https://github.com/ExpDev07/legacy-rp-admin-v3/issues" target="blank">
        <img src="https://img.shields.io/github/issues/ExpDev07/legacy-rp-admin-v3" alt="GitHub issues">
    </a>
</p>

<hr>

<p>
    <a href='https://ko-fi.com/C1C510DUQ' target='_blank'>
	<img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi3.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' />
    </a>
</p>

<strong>100% FREE + OPEN SOURCE.</strong>

> A web interface to help with administrative duties at the FiveM server: Legacy Roleplay. Written in PHP using [Laravel Framework](https://laravel.com/) and
> [Tailwindcss](https://tailwindcss.com) for the frontend.

See [#contributing](#Contributing) for more details on how you can help shape **legacy-rp-admin-v3**. We're always down to improve and receive feedback.

**Urgent? Write to me on Discord: Marius Big Ounce#2997.**

## Note

It is recommended that you use Chrome or Edge as your web browser for the best experience.

## Features
* See and search server logs.
* See and search players.
* Warn and ban players, also temporarily.
* See and edit characters.
* [*... open issue to request a feature.*](https://github.com/ExpDev07/legacy-rp-admin-v3/issues/new/choose)

## Improvements from v1
1. Re-done to give a more minimalistic and "simple" look.
2. Backend improvements that encourage scaling and maintainability. 
3. More.

## License
Please refer to [LICENSE.md](https://github.com/ExpDev07/legacy-rp-admin-v3/blob/master/LICENSE.md) for this project's license.

## Contributors
This list only contains some of the most notable contributors. For the full list, refer to [GitHub's contributors graph](https://github.com/ExpDev07/legacy-rp-admin-v3/graphs/contributors).
* ExpDev07 (Marius) - creator and maintainer of both frontend and backend.

## Pictures
(more will be posted soon once some design decisions are finalized).

### Logging in
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/logging_in.PNG" alt="Logging in" />

### Dashboard
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/dashboard.PNG" alt="Dashboard" />

### Players
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/players.PNG" alt="Players" />

### Viewing player
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/player.PNG" alt="View Player" />

### Viewing character
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/character.PNG" alt="View Character" />

### Server Logs
<img src="https://github.com/ExpDev07/legacy-rp-admin-v3/raw/master/.github/screenshots/logs.PNG" alt="Logs" />

## Contributing
This section describes how you can help contribute.

### Prerequisites
* PHP 7.4+.
* Composer.
* Node (and npm).
* SQL (database).

### Setting up project
Grab yourself a copy of this repository:
```bash
$ git clone https://github.com/ExpDev07/legacy-rp-admin-v3.git
```

Install all the required dependencies (we use both npm and composer):
```bash
$ composer install
$ npm install
```

Create a new file called ``.env`` and copy the contents from ``.env.example`` over to it, then apply your configurations.
```bash
$ cp .env.example .env
```

Create a private and unique application key:
```bash
$ php artisan key:generate
```

Run database migrations so that we can store things:
```bash
$ php artisan migrate
```

Create a symbolic link at **public/storage** so that it points to **storage/app/public**:
```bash
$ php artisan storage:link
```

Compile frontend assets (use "dev" for development and "prod" for production):
```bash
$ npm run dev/prod
```

Finally, boot the server up:
```bash
$ php artisan serve
```
