# Ebfly

Ebfly is a simple command line interface for [AWS ElasticBeanstalk](http://aws.amazon.com/en/elasticbeanstalk/).

## Prerequisities

- Ruby 1.9+
- git

## Install

Ebfly can be install via gem:

```
$ gem install ebfly
```

After install, you can use `ebfly` command.

## Setup

You need to create `$HOME/.ebfly` and specify following values.

```
AWS_ACCESS_KEY_ID='...'
AWS_SECRET_ACCESS_KEY='...'
AWS_REGION='us-east-1'
```

or you can also specify these values via `ENV`:

```
export AWS_ACCESS_KEY_ID='...'
export AWS_SECRET_ACCESS_KEY='...'
export AWS_REGION='us-east-1'
```

## Quick Start

If you want to deploy sinatra app, you should do following step.

### Step 1 Create git repository

Create working directory, and create git repository.

```sh
$ mkdir sinatraapp
$ cd sinatraapp
$ git init .
```

### Step 2 Create application

Create application using `ebfly app create` command.

```sh
# Usage: "ebfly app create <application-name>"
$ ebfly app create sinatraapp
Create app: sinatraapp ...

=== application info ===
application name: sinatraapp
description:
created at:       2014-02-25 08:43:30 UTC
updated at:       application name: 2014-02-25 08:43:30 UTC
configuration_templates:
```

### Step 3 Create environment

Create environment using 'ebfly env create' command.

```sh
# Usage: "ebfly env create <environment-name> -a <application-name> -s <solution_stack_name> -t <tier-type>
$ ebfly env create dev -a sinatraapp -s ruby19 -t web
Create environment: sinatraapp-dev ...

=== environment info ===
application name:    sinatraapp
environment id:      e-XXX
environment name:    sinatraapp-dev
description:
status:              Launching
health:              Grey
tier:                WebServer Standard 1.0
solution stack name: 64bit Amazon Linux 2013.09 running Ruby 1.9.3
updated at:          2014-02-25 08:45:39 UTC
```

### Step 4 Create sinatraapp

Create `config.ru`, `helloworld.rb` and `Gemfile` like this:

#### config.ru

```rb
require './helloworld'
run Sinatra::Application
```

#### helloworld.rb

```rb
require 'sinatra'
get '/' do
  "Hello World!"
end
```

#### Gemfile

```rb
source 'http://rubygems.org'
gem 'sinatra'
```

### Step 5 Bundle install and commit working files

```sh
$ bundle install
$ git add .
$ git commit -m "First commit"
```

### Step 6 Deploy app

Deploy app's `master` branch to environment using `ebfly env push` command.

```sh
# Usage: "ebfly push <name> <branch or tree_ish>".
$ ebfly env push dev master -a sinatraapp

=== environment info ===
application name: sinatraapp
environment id:   e-wwdh3u39bg
environment name: sinatraapp-dev
description:
status:     Ready
health:     Green
tier:     WebServer Standard 1.0
solution stack name:  64bit Amazon Linux 2013.09 running Ruby 1.9.3
endpoint url:   awseb-e-w-AWSEBLoa-18QJHE7KJ67YX-1070649090.us-east-1.elb.amazonaws.com
cname:      sinatraapp-dev-esfmkp3rgk.elasticbeanstalk.com
updated at:   2014-02-25 08:50:51 UTC

Create archive: git-8e81464455269535c89149643ba50fab5cfa82da-1393318559.zip
Upload archive git-8e81464455269535c89149643ba50fab5cfa82da-1393318559.zip to s3://elasticbeanstalk-us-east-1-283381710361
Create application version: 8e81464455269535c89149643ba50fab5cfa82da-1393318559
Update environment: sinatraapp-dev to 8e81464455269535c89149643ba50fab5cfa82da-1393318559

=== environment info ===
application name: sinatraapp
environment id:   e-wwdh3u39bg
environment name: sinatraapp-dev
description:
status:     Updating
health:     Grey
tier:     WebServer Standard 1.0
solution stack name:  64bit Amazon Linux 2013.09 running Ruby 1.9.3
endpoint url:   awseb-e-w-AWSEBLoa-18QJHE7KJ67YX-1070649090.us-east-1.elb.amazonaws.com
cname:      sinatraapp-dev-esfmkp3rgk.elasticbeanstalk.com
version label:    8e81464455269535c89149643ba50fab5cfa82da-1393318559
updated at:   2014-02-25 08:56:01 UTC
```

### Step 7 Open app



## License

This tool is distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
