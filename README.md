<h1 align="center">
  <a href="https://kiln.db"><img src="./assets/kiln.png" alt="kiln" height="250px"></a>
  <br>
  KilnDB Examples
  <br>
</h1>
<h4 align="center">A all-in-one platform for hosting your own databases and CDN, giving you control over your data and content.</h4>

### Purpose

The purpose of this repository is to give examples on how certain parts of the platform works.

#### Including:

##### Apps

Apps are containers ran by Docker. These apps are ran in production with three specific files they require to run, including `Dockerfile`, `docker-compose.yml` (basic Docker necessities), and a `kiln.yml` or `kiln.toml` (required to select the ports to open). 

###### Kiln Configuration File

Our configuration file is `kiln.yml` and `kiln.toml`, we use these to select the ports to open and use with your app. The `kiln.yml` should be like this. 

```
run:
  port: 3000
  env:
    NODE_ENV: production
resources:
  memory: 512m
  cpu: 0.5
```

Your Kiln configuration file is required otherwise it will not launch and return in your build log with

```
[kiln] Failed: No kiln.yml found at the root of the repo
```

but if your Kiln configuration file was found, then it will return 

```
[kiln] Found kiln.yml (port ****)
```

with `port` returned from your configuration file.

#### Databases

The databases we offer act as normal databases as you would use on your machine. 

##### Usage

We prefer using the API to talk to your database. It would work with 

```
https://api.kilndb.com/db/{id}/query
```

must include the header 

```
Authorization: Bearer kiln_YOUR_KEY
```

You can get your token at /dashboard/api-key. Then you would send your request as a body. For PostgreSQL, you would do

```
{
  "sql": "SELECT * FROM users WHERE id = $1",
  "params": [
    1
  ]
}
```

as a curl request you would send as

```
curl -X POST "https://api.kilndb.com/db/a1b2c3d4e5/query" \
  -H "Authorization: Bearer kiln_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"sql":"SELECT * FROM users WHERE id = $1","params":[1]}'
```

If successful (200), it will return 

```
{
  "rows": [
    {
      "id": 1,
      "name": "Brody"
    }
  ],
  "count": 1,
  "truncated": false
}
```

If not found, it will return

```
{
  "error": "Not found"
}
```

