## Docs

If you'd like to check out what Slate is capable of, take a look at the [sample docs](http://lord.github.io/slate)

## Getting Started with Slate 

### Prerequisites

You're going to need:

 - **Linux or macOS** — Windows may work, but is unsupported.
 - **Ruby, version 2.3.1 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

## Starting the Server

Locally

```shell
# either run this to run locally
bundle install
bundle exec middleman server

# OR run this to run with vagrant
vagrant up
```

Using docker

```shell
docker-compose up
```

You can now see the docs at http://localhost:4567.

## Editing the Documents

You can edit the documents by editing the following file: `source/index/html.md`

## Deployment

Run the following command to push the changes to https://docs.getslideapp.com/. 

```
./deploy.sh
```
