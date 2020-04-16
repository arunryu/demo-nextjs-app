# Deployment on Heroku (optional)

We recommend deployment on `now.sh` for next.js apps. However, there may be some reasons to prefer Heroku to `now.sh:

- You may want to use a Postgres SQL database (provided free with Heroku, up to 10,000 rows)
- Perhaps you just prefer Heroku because you have prior experience with it

If you do want to proceed with Heroku, here's what to do.

These instructions assume that you have already (a) created a Heroku.com account, (b) have
the Heroku CLI installed on your system (it is already installed on CSIL).

1. Create a new app on Heroku, either via the Heroku Dashboard, or the Heroku command line.
   For purposes of the instructions, let us suppose this is called `cs48-s20-cgaucho-lab00`

2. Add a value for `SESSION_COOKIE_SECRET` to to your `.env` file.

   The value can be any arbitrary string of upper and lower case letters and digits.
   It is just a value used to encrypt your session cookies so that it's more difficult
   for hackers to hijack your session.

   Example:

   ```
   SESSION_COOKIE_SECRET=7xd6fvweSFHSS238778sf87sdfS8F8sf9ds8fDZ7sd8fdDV8ASC1232
   ```

3. Be sure you are logged into the Heroku command line via `heroku login -i`

4. Run `npm install --global heroku-dotenv`

   This is a utilty that provides the ability to push or pull values from `.env` to the
   Heroku Config Vars for a Heroku app.

   (On CSIL, follow instructions [here](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md) before you try to use `npm install --global ...`. Note that the `.bashrc` file you need to edit is `${HOME}/.bashrc`.)

5. Run this command to copy the `.env` values into Heroku Config Vars. Substitute your app
   name in place of `cs48-s20-cgaucho-lab00`

   ```
   heroku-dotenv push --app cs48-s20-cgaucho-lab00
   ```

6. Go to the Deploy screen of the Heroku Dashboard, connect your GitHub repo
   to the Heroku App, and then click to deploy the master branch.

7. You should be up and running on Heroku. However, you still still need to modify your
   Auth0 setup to include the new production urls.

# Setting up Auth0 for your running Heroku app.

# Setting up Auth0 for Heroku (production)

In order for Auth0 to recognize the app running on a production url
running on heroku, you will need to make a small change to the Auth0
configuration you did to get set up on `localhost`.

Navigate back to the settings page of the app you created in the Auth0
dashboard.

To do this:

- return to the web interface of <https://auth0.com/> and login
- click on `Applications` in the side menu
- select your application
- go to the second tab for `Settings`

Your production url is something of the form

```
https://cs48-s20-cgaucho-lab00.herokuapp.com
```

For every field that references `http://localhost:3000`:

- Add a comma-separated entry after the existing entry referencing your new production url.
- It is important you include **both** `localhost` **and** production urls so that both your localhost and production apps will work properly.

For example, if your production url is `https://cs48-s20-cgaucho-lab00.herokuapp.com`,
your fields should now look like this.

Allowed Callback URLs:

```
http://localhost:3000/api/callback, https://cs48-s20-cgaucho-lab00.herokuapp.com/api/callback
```

Allowed Logout URLs:

```
http://localhost:3000, https://cs48-s20-cgaucho-lab00.herokuapp.com
```

Notes:

- Be sure that the `localhost` values use `http` but the `now.sh` values use `https`
- Don't just copy the above values; replace `https://cs48-s20-cgaucho-lab00.herokuapp.com` with the link to your own deployment of the production app.
- If you want to have both a `now.sh` deployment and a heroku deployment, it is fine to have all three urls listed (localhost, `now.sh` and heroku).

Don't forget to scroll down and click `Save Changes` at the bottom of the page.

## Final Step

Go to the Setting page in the Heroku Dashboard and add two config vars:

| Key                        | Value                                             | Example                                                     |
| -------------------------- | ------------------------------------------------- | ----------------------------------------------------------- |
| `REDIRECT_URI`             | Your production URL with `/api/callback` appended | `https://cs48-s20-cgaucho-lab00.herokuapp.com/api/callback` |
| `POST_LOGOUT_REDIRECT_URI` | Your production URL                               | `https://cs48-s20-cgaucho-lab00.herokuapp.com/api/callback` |

Once you've defined these, redeploy your app, and it should work on Heroku. Be sure that you don't only test loading the home page, but also test whether you can authenticate (login/logout with Google).

# Next step

Return to [README.md](../README.md)
