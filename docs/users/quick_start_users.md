## Quick Start for Users

This a quick start guide, if you are interested in a more comprehensive guide please refer to the [User Complete Guide](user_guide.md)

1. Make sure you meet the pre-requisites on your machine:
    * Python 3.9+ (and `pip`) 
    * Docker 20+ with Compose v2
    * Git
    
2. install the RAPyDo controller

    Install the latest version: `pip3 install --upgrade rapydo

    You have now the executable `rapydo` (you can try a `rapydo --version`). Your project could require a different version, in this case you will be able to install the right version once configured your project

    If installed as user (i.e. not with `sudo` or with a root user) you may find the rapydo binary in `$HOME/.local/bin` instead of `/usr/bin` or `/usr/local/bin`. Make sure that the path is included in your `$PATH` environment variable, otherwise you will end up with `command not found`. Please consider to add to your `~/.bashrc`:

    ```bash
    if [ -d "$HOME/.local/bin" ] ; then
        PATH="$HOME/.local/bin:$PATH"
    fi
    ```

3. clone and initialize your project

```bash
git clone https://git.../your_project/example.git
git checkout your_branch
rapydo init
```

3. Customize your `.projectrc` file. By editing this file you can override all options of your project for your specific deployment (e.g. set custom passwords for deployed services)

4. If your project requires it, install a specific RAPyDo controller version

   You can verify the required version with `rapydo version`

   You can install the version required by your project with: `rapydo install`

5. Use the controller to start your project

```bash
# pull base images
rapydo pull
# launch the containers
rapydo start
```

5. Launch the server in debug mode

```bash
# check the containers are running
rapydo status
# start the backend default command in interactive mode
rapydo shell backend --default
# Please note that this command is equivalent to:
# rapydo shell backend "restapi launch"

[...]
# lots of logs
# if you need to stop it use CTRL+C
```

6. Test your server

The port `8080` should be accessible now, you can query it by using a a http client (curl, wget, `httpie` [HTTPie] (https://httpie.org/) from another shell, and checking the logs in the other one.

```bash
# test a normal endpoint with no authentication
http GET localhost:8080/api/status

# request a token to the server with a default account
# the account is available only in debug mode
http POST localhost:8080/auth/login username=user@nomail.org password=XXX

# save the token from the previous response
export TOKEN="..."

# call an authenticated endpoint helper, e.g. check the profile
http GET localhost:8080/auth/profile Authorization:"Bearer $TOKEN"
# "status": "Valid user"
```

7. check the swagger definitions

Your server automatically provides an API definition compliant to the [OpenAPI standard]() at the URI:
http://localhost:8080/api/swagger

And you may launch a swagger interface container to access your current API definition and play with it:

```bash
rapydo run swagger

[...]
You can access swaggerui web page here:
http://localhost?docExpansion=none
```

## Clean up

You can remove your containers (by keeping all data) with

```
rapydo remove
```

To also remove persistent data stored in docker volumes:

```
rapydo remove --all
```

