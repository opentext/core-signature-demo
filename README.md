# OT2 Reference Application for Core Signature
This is a demo application which shows how you can build an application that leverages the Core Signature API to manage document signature flows. This is a lightweight frontend application built on the VueJS framework.

## Project Setup for the Frontend
Setting up this application is simple - once cloned, [install NodeJS](https://nodejs.org/en/download/package-manager/) if you haven't already then simply do the following:

1. Add `signature.demo` to the `127.0.0.1` entry in your hosts file.
2. Run `npm install` to install dependencies
3. Run `npm run serve` to start the application

That's it! This will install necessary dependencies and compile the application with hot-reloading. The application will be available at `signature.demo:8080` in your browser.

In addition, you are able to:
* Compile and minify for production using `npm run build`
* Run the linter using `npm run lint`

## Project Setup for the Backend
To run the facade server you need to have Python 3 version greater than 3.4 installed. This will automamically have pip installed. If you don't have pip already installed, follow instructions [here](https://pip.pypa.io/en/stable/installation/) (for MacOS and Windows users).
You'll want to run a second terminal window for the backend.
Install pipenv using `pip install pipenv` for Windows users or `brew install pipenv` for Mac user using Homebrew. More information on pipenv installation can be found [here](https://pypi.org/project/pipenv/).
Install other dependencies from the Pipfile using `pipenv install`.

The following env var information has been added for convenience to the .env file in the root:
```bash
FLASK_APP=app_server.py
FLASK_ENV=development
FLASK_RUN_HOST=signature.demo
API_SERVER_HOST=https://sign.core.opentext.com/
```

Run `pipenv shell`
To start the server run the following command from the root of the project while in the shell `make server`

## Example JSON credential file format:
```json
{
  "site": "your_site",
  "clientID": "your_oauth_client_id",
  "clientSecret": "your_oauth_client_secret",
  "subscription": "subscription:coresignature-signingsite-123213",
  "username": "your_email@test.com",
  "password": "your_password",
  "authHost": "https://otdsauth.ot2.opentext.com",
  "apiHost": " http://signature.demo:5000/,
}
```

## FAQ

**Can I customize the application's host?**

Yes, the default host of `signature.demo` can be set to anything you'd like by modifying the `devServer.host` property in the `vue.config.js` file. You can confirm your changes have taken effect after running `npm run serve`.

**Do I have to use a password grant for authentication with OT2?**

No, this demo application uses a password grant as an example. When using OT2 authentication directly with OTDS, all OAuth2 grant types are supported. A great resource on the ways you can obtain your access token [can be found here](https://developer.opentext.com/resources/documentation/Manage%20and%20Secure/0c60d99343c11b3025ce4f05adca3e59/page/6).

**Is polling the only way I can retrieve the final document?**

No, we support several methods of document retrieval. You can use the `/documents/` API endpoint to retrieve a document regardless of where in a signature flow it is. Additionally, Core signature supports webhooks, so you can register a listener on your backend as a webhook using our `/webhooks/` API endpoint. If you have a webhook registered for an event, we will asynchronously deliver the event to you, no polling needed! More information can be found in our [official API documentation](https://sign.core.opentext.com/api/v1/docs/).

**Do I need to have a subscription to use Core Signature?**

Yes, when authenticating with OT2 you will need to include the OT2 subscription provided to you on purchase when obtaining your access token.

**I'd really like to see an example of X. Will this app be augmented over time?**

Absolutely! This app will be regularly improved with more API usage and features. If you have specific requests for which you'd like to see example implementations, reach out to us. For developers with access to developer.opentext.com, you can do so there, or you can always reach out to your OpenText representative and let them know.
