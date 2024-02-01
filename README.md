# Arbiteus

Arbiteus provides a quick and easy way to use your own machines for hosting your web apps.

Whether you are prototyping a new idea and want an internet addressable way to reach your app running on localhost or you have an existing web app that you want to host across your multiple spare machines instead of paying cloud providers for expensive VMs, we've got you covered.

## Assumptions

You have a web app running on atleast one your machines on a well defined port and is accessible via browser or curl using ```http://localhost:\{your port}``` or ```https://localhost:\{your port}```

You have outbound connectivity from your machine(s).

You are using a Windows machine.

## Get Started

Download the latest CLI for using Arbiteus on Windows : ```https://arbiteus.blob.core.windows.net/windows-cli/Arbiteus.CLI.exe```

Open a terminal or powershell and navigate to the folder where you downloaded the CLI

### Check CLI Version

You can check the CLI version using the following command on your terminal

```.\Arbiteus.CLI.exe version```

You should see a response like :

```
Using endpoint : https://arbiteus.net
Version : 1.0
```

### Create your account

If you are using Arbiteus for the first time you will need to create an account first.

We use Google as our primary account provider.

```.\Arbiteus.CLI.exe account create```

A web page should open up in your browser asking you to login to your Google account.

**Note** : This page is owned by Google and we do not see or get any private information (like the password you enter) from this page.

Once you login successfully, Google sends us the following information via a Google signed auth token, which we use to create your account :

- Name
- Email

Thats it!. We don't keep any private information nor add any additional cookies for tracking.

Once you login, you should see a response like :

```json
{
  "id": "c3VwZXJtYW4xMjNAZ21haWwuY29t",
  "email": "superman123@gmail.com",
  "apiKey": "d5c35bc5-279f-4a82-bdcc-54d305d7a4ee",
  "accountTier": "Free",
  "createdAt": "2024-01-22T22:43:01.7297368+00:00",
  "lastUpdatedAt": "2024-01-22T22:43:01.7298952+00:00",
  "UpdateReason": "AccountCreated"
}
```

### Create a Web App

Assuming you have your web app running on port 7000 (could be any port, this is just an example)

You can create your web app using the following command :
```
.\Arbiteus.CLI.exe webapps create --name supermansampleapp --description "First app" --lang "c#" --port 7000 --protocol Https
```

Make sure you are using the right protocol value depening on your app - Https or Http

You should see a response like this if your web app got created : 
```json
{
  "port": 7000,
  "protocol": "Https",
  "accountId": "c3VwZXJtYW4xMjNAZ21haWwuY29t",
  "applicationName": "supermansampleapp",
  "language": "c#",
  "description": "First app",
  "type": "StatelessAPI",
  "state": "Enabled",
  "createdAt": "2024-01-30T08:04:05.1430586+00:00",
  "lastUpdatedAt": "2024-01-30T08:04:05.1430594+00:00",
  "UpdateReason": "Creation"
}
```

### Start a worker on your machine

Start a worker that connects to your local app
```
 .\Arbiteus.CLI.exe worker start --appName supermansampleapp
```

### Create a shorthand url for your web app

Now that your web app is linked with Arbiteus, you need a internet facing url that can reach your web app running on localhost from anywhere on the web.

You can create a public link to your app as follows :
```
.\Arbiteus.CLI.exe webapps shorthand create --name supermansampleapp --handle supermansampleapp
```

You should see a successful response like this : 
```json
{
  "handle": "supermansampleapp",
  "appName": "supermansampleapp",
  "accountId": "c3VwZXJtYW4xMjNAZ21haWwuY29t",
  "createdAt": "2024-02-01T20:15:33.3451052+00:00",
  "lastUpdatedAt": "2024-02-01T20:15:33.3452306+00:00",
  "UpdateReason": "Creation"
}
```

This means that now your local app running at : ```https://localhost:7000```
is accessible from the web using the url : ```https://supermansampleapp.arbiteus.net ```

Lets assume you have an api path on your web app like : 
```https://localhost:7000/api/ping```
This should now be accessible from the web like : 
```https://supermansampleapp.arbiteus.net/api/ping```

### Distributed Web App

If you have more than 1 machine that has your web app running at localhost:7000 and you want to connect all your machines together as a single virtual server, you need to : 

1. Download the Arbiteus CLI on these machines
2. Start workers for your web app on these machines

### Limits

On a free account, the following limits apply : 

1. Number of Web Apps Per Account : 3
1. Number of Workers Per Web App : 3
1. Number of Shortands Per Account : 3
1. Number Requests/Second your Web App can service without throttling : 10

### Features Coming Soon

1. Support for MacOS
2. Ability to deploy your updated binaries from your main development machine to all your supporting machines where workers are running.
3. Support for Stateful Services.
4. Replication for data folders across all your machines where workers are running.
5. Support for additional types of app - 
	1. Command Center (run a terminal or powershell command on all your machines)
	2. Workflow apps - Don't want to write complicated Spark and other Map Reduce jobs, you can use simple create a workflow that is serviced by your own machines and do data processing.
6. Logs from all your machines accessible in a single place so you can debug request failures easily.



