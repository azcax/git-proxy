Git-Proxy
=========

It is used as a cloud bridge between your web application and Github, especially if you want to get access to [Github API](https://developer.github.com/v3/) completely inside the browser, without any backend-side in your application. For example, if you want to deeply communicate with Github API from your Github Pages. 

In this case Github API have some [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)
limitations and we encounter with a lot of difficulties to provide multi-step [OAuth](https://developer.github.com/v3/oauth/) process. The problem is described in detail in this [great post](http://blog.vjeux.com/2012/javascript/github-oauth-login-browser-side.html).

Using a lightweight proxy server we can avoid all this troubles and also we can safely store all our sensitive data. 

> Actually, you should install the proxy **only once**, and then use it for all your web applications that you want to provide a simple access to Github API. The only thing to do is to add the application sensitive data to server environmental variable. And server will know about the application.

##Install and configure

To use it as your own proxy you should fork this repo. Then clone it locally and configure:
```
$git clone https://github.com/<your_name>/git-proxy.git
$cd git-proxy
$touch application.json
```
In the created `application.json` file you should add credentials for your applications. A common view of `application.json` file is
```json
//application.json
{
	"production": {
		"your_app1": {
			"client_id": "iug45jhgejrgtwy4ugtw348",
			"client_secret": "kjh34kh5wiueyr9s84ukjh3543u4r87er8ch2eiu"
		},
		"your_app2": {
			"client_id": "kl234l2j3rio2jro2iej2o2",
			"client_secret": "weroi323ur29jowej29r2o2pej2oiej2oiejr2ie"
		}
		//and so on
	},
	"development": {
	    "your_dev_app": {
			"client_id": "dfkjhk45h3k4j3kjr3k4h34",
			"client_secret": "ero4irj2io32oi3ur20r2093r2orjo2irjio23jr"
		}
		//and so on
	}
}
```
Comments should be removed from application.json in case. 

> **NOTE:** Add your `application.json` to `.gitignore`. It is important to keep your sensitive data private.

Next steps =>

##Deploying to Heroku

Git-proxy uses Heroku as a primary cloud service without additional adjustments. So, if you are [free](https://devcenter.heroku.com/articles/quickstart) with Heroku, create an app:
```
$heroku apps:create your-git-proxy
```
If you make some changes in the repo, you can add it to github and then deploy your project to Heroku:
```
$git push heroku master
```
And the final step is to configure your environment variables in the cloud:
```
$bash update.sh
```
After that your proxy will know about your sensitive data.

##Updating

If you want to update some info about applications `client_id` and `client_secret` on proxy, just update `application.json` file and run
```
$bash update.sh
```

##Usage

After git-proxy will run successfully, your applications can get github authentication token via following request 
```
REQUEST:
    GET http://your-git-proxy.herokuapp.com/github_access_token?client_id=<client_id>&code=<session_code>

RESPONSE:
    {"access_token": <access_token>, "token_type": <token_type>, "scope": <scope>}
```

## Register Application

To register the web application you should go to your `Account settings` -> `Application` tab -> click `Register new application` button -> and fill the form with right URLs. 

After that Github will generate `client_id` and `client_secret` for your app.

---
[MIT License](https://github.com/krispo/git-proxy/blob/master/LICENSE)